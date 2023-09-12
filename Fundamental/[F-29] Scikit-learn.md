

# Intro[¶](#Intro)

**Aim**

-   Learn basic concept of Recommendation using scikit-learn

**Contents**

-   Recommendation system
-   Similarity
-   Types of Recommendation system
    -   Content Based Filtering
    -   Collaborative Filtering
        -   User base
        -   Item base
        -   Latent Factor Collaborative Filtering -> matrix factorization

```
# making directory
$ mkdir -p ~/aiffel/movie_recommendation
```

# Recommendation system[¶](#Recommendation-system)

Recommendation system: a subclass of information filtering system that seeks to predict the "rating" or "preference" a user would give to an item.

-   Deals with categorical data: discrete!
-   Calculate Similarity after transforming to numerical vector: In order to represent categorical data in coordinate, we have to transform to numerical vector.

# Cosine Similarity[¶](#Cosine-Similarity)

Cosine Similarity Calculates the similarity by calculating cosine value between two vectors.

![image](https://user-images.githubusercontent.com/78291267/154893992-f58dfa7e-55e4-4663-ba51-c581a71849d1.png)![image](https://user-images.githubusercontent.com/78291267/154894021-3b790cc0-03e5-4f55-8380-97fde5684693.png)

Let's say we have numerical vectors and calculate cosine similarity.

In \[21\]:

```
# using numpy
import numpy as np

t1 = np.array([1, 1, 1])
t2 = np.array([2, 0, 1])

from numpy import dot
from numpy.linalg import norm
def cos_sim(A, B):
	return dot(A, B)/(norm(A)*norm(B))

cos_sim(t1, t2)
```

Out\[21\]:

```
0.7745966692414834
```

In \[22\]:

```
# using scikit learn: easier!
from sklearn.metrics.pairwise import cosine_similarity

t1 = np.array([[1, 1, 1]])
t2 = np.array([[2, 0, 1]])
cosine_similarity(t1,t2)
```

Out\[22\]:

```
array([[0.77459667]])
```

# Types of Recommendation system[¶](#Types-of-Recommendation-system)

-   Content Based Filtering
-   Collaborative Filtering
    -   User base
    -   Item base
    -   Latent Factor Collaborative Filtering -> matrix factorization

We'll look around `Content Based filtering` and `Collaborative filtering`.

![image](https://user-images.githubusercontent.com/78291267/154894338-216b3b8d-612f-4db5-a081-7fc1ce6a44eb.png)

# Content Based Filtering[¶](#Content-Based-Filtering)

`Content-based filtering` uses item features to recommend other items similar to what the user likes, based on their previous actions or explicit feedback.

We select movie with some standards like genre, actor, producer, etc. These are the **feature** of a movie, and we can say that 'contents' are similar.

[cite : 'Building a Movie Recemmendation Engine in Python using Scikit-Learn'](http://www.codeheroku.com/post.html?name=Building%20a%20Movie%20Recommendation%20Engine%20in%20Python%20using%20Scikit-Learn)

## 1) import module[¶](#1)-import-module)

In \[6\]:

```
import pandas as pd
import numpy as np
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity

print('⏩⏩')
```

```
⏩⏩
```

## 2) Load data[¶](#2)-Load-data)

```
get https://aiffelstaticprd.blob.core.windows.net/media/documents/movie_dataset.csv
mv movie_dataset.csv  ~/aiffel/movie_recommendation
```

In \[7\]:

```
import os
csv_path = os.getenv('HOME')+'/aiffel/movie_recommendation/movie_dataset.csv'
df = pd.read_csv(csv_path)
df.head()
```

Out\[7\]:

|   | index | budget | genres | homepage | id | keywords | original\_language | original\_title | overview | popularity | ... | runtime | spoken\_languages | status | tagline | title | vote\_average | vote\_count | cast | crew | director |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | 237000000 | Action Adventure Fantasy Science Fiction | http://www.avatarmovie.com/ | 19995 | culture clash future space war space colony so... | en | Avatar | In the 22nd century, a paraplegic Marine is di... | 150.437577 | ... | 162.0 | \[{"iso\_639\_1": "en", "name": "English"}, {"iso... | Released | Enter the World of Pandora. | Avatar | 7.2 | 11800 | Sam Worthington Zoe Saldana Sigourney Weaver S... | \[{'name': 'Stephen E. Rivkin', 'gender': 0, 'd... | James Cameron |
| 1 | 1 | 300000000 | Adventure Fantasy Action | http://disney.go.com/disneypictures/pirates/ | 285 | ocean drug abuse exotic island east india trad... | en | Pirates of the Caribbean: At World's End | Captain Barbossa, long believed to be dead, ha... | 139.082615 | ... | 169.0 | \[{"iso\_639\_1": "en", "name": "English"}\] | Released | At the end of the world, the adventure begins. | Pirates of the Caribbean: At World's End | 6.9 | 4500 | Johnny Depp Orlando Bloom Keira Knightley Stel... | \[{'name': 'Dariusz Wolski', 'gender': 2, 'depa... | Gore Verbinski |
| 2 | 2 | 245000000 | Action Adventure Crime | http://www.sonypictures.com/movies/spectre/ | 206647 | spy based on novel secret agent sequel mi6 | en | Spectre | A cryptic message from Bond’s past sends him o... | 107.376788 | ... | 148.0 | \[{"iso\_639\_1": "fr", "name": "Fran\\u00e7ais"},... | Released | A Plan No One Escapes | Spectre | 6.3 | 4466 | Daniel Craig Christoph Waltz L\\u00e9a Seydoux ... | \[{'name': 'Thomas Newman', 'gender': 2, 'depar... | Sam Mendes |
| 3 | 3 | 250000000 | Action Crime Drama Thriller | http://www.thedarkknightrises.com/ | 49026 | dc comics crime fighter terrorist secret ident... | en | The Dark Knight Rises | Following the death of District Attorney Harve... | 112.312950 | ... | 165.0 | \[{"iso\_639\_1": "en", "name": "English"}\] | Released | The Legend Ends | The Dark Knight Rises | 7.6 | 9106 | Christian Bale Michael Caine Gary Oldman Anne ... | \[{'name': 'Hans Zimmer', 'gender': 2, 'departm... | Christopher Nolan |
| 4 | 4 | 260000000 | Action Adventure Science Fiction | http://movies.disney.com/john-carter | 49529 | based on novel mars medallion space travel pri... | en | John Carter | John Carter is a war-weary, former military ca... | 43.926995 | ... | 132.0 | \[{"iso\_639\_1": "en", "name": "English"}\] | Released | Lost in our world, found in another. | John Carter | 6.1 | 2124 | Taylor Kitsch Lynn Collins Samantha Morton Wil... | \[{'name': 'Andrew Stanton', 'gender': 2, 'depa... | Andrew Stanton |

5 rows × 24 columns

## 3) Select Features[¶](#3)-Select-Features)

In \[8\]:

```
df.columns
```

Out\[8\]:

```
Index(['index', 'budget', 'genres', 'homepage', 'id', 'keywords',
       'original_language', 'original_title', 'overview', 'popularity',
       'production_companies', 'production_countries', 'release_date',
       'revenue', 'runtime', 'spoken_languages', 'status', 'tagline', 'title',
       'vote_average', 'vote_count', 'cast', 'crew', 'director'],
      dtype='object')
```

In \[9\]:

```
# select features
features = ['keywords','cast','genres','director']
features
```

Out\[9\]:

```
['keywords', 'cast', 'genres', 'director']
```

In \[10\]:

```
def combine_features(row):
    return row['keywords']+" "+row['cast']+" "+row['genres']+" "+row['director']

combine_features(df[:5])
```

Out\[10\]:

```
0    culture clash future space war space colony so...
1    ocean drug abuse exotic island east india trad...
2    spy based on novel secret agent sequel mi6 Dan...
3    dc comics crime fighter terrorist secret ident...
4    based on novel mars medallion space travel pri...
dtype: object
```

In \[11\]:

```
for feature in features:
    df[feature] = df[feature].fillna('')

df["combined_features"] = df.apply(combine_features,axis=1)
df["combined_features"]
```

Out\[11\]:

```
0       culture clash future space war space colony so...
1       ocean drug abuse exotic island east india trad...
2       spy based on novel secret agent sequel mi6 Dan...
3       dc comics crime fighter terrorist secret ident...
4       based on novel mars medallion space travel pri...
                              ...                        
4798    united states\u2013mexico barrier legs arms pa...
4799     Edward Burns Kerry Bish\u00e9 Marsha Dietlein...
4800    date love at first sight narration investigati...
4801     Daniel Henney Eliza Coupe Bill Paxton Alan Ru...
4802    obsession camcorder crush dream girl Drew Barr...
Name: combined_features, Length: 4803, dtype: object
```

## 4) Vectorize, Cosine Similarity[¶](#4)-Vectorize,-Cosine-Similarity)

Now let's calculate the cosine similarity using `CountVectorizer()` in scikit-learn

In \[12\]:

```
cv = CountVectorizer()
count_matrix = cv.fit_transform(df["combined_features"])
print(type(count_matrix))
print(count_matrix.shape)
print(count_matrix)
```

```
<class 'scipy.sparse.csr.csr_matrix'>
(4803, 14845)
  (0, 3115)	1
  (0, 2616)	1
  (0, 4886)	1
  (0, 12386)	2
  (0, 14235)	1
  (0, 2755)	1
  (0, 12299)	1
  (0, 11517)	1
  (0, 14561)	1
  (0, 14820)	1
  (0, 11490)	1
  (0, 12134)	1
  (0, 14291)	1
  (0, 12567)	1
  (0, 7496)	1
  (0, 8831)	1
  (0, 11217)	1
  (0, 86)	1
  (0, 144)	1
  (0, 4435)	1
  (0, 11745)	1
  (0, 4566)	1
  (0, 6542)	1
  (0, 2061)	1
  (1, 86)	1
  :	:
  (4801, 10069)	1
  (4801, 5844)	1
  (4801, 252)	1
  (4801, 4098)	1
  (4801, 14796)	1
  (4801, 11361)	1
  (4801, 2978)	1
  (4801, 12036)	1
  (4801, 6138)	1
  (4802, 9659)	1
  (4802, 3812)	1
  (4802, 1788)	2
  (4802, 4210)	1
  (4802, 5181)	1
  (4802, 2912)	1
  (4802, 3821)	1
  (4802, 1069)	1
  (4802, 11185)	1
  (4802, 3681)	1
  (4802, 5399)	1
  (4802, 3894)	1
  (4802, 2056)	1
  (4802, 3093)	1
  (4802, 4502)	1
  (4802, 5900)	2
```

We can see that `count_matrix`'s type is **CSR(Compressed Sparse Row) Matrix**.

**CSR(Compressed Sparse Row) Matrix** is a data structure which can save memory and represent the same matrix by making up not 0 data but valid value of data,

Every moive was vectorized. Now let's calculate cosine\_simimlarity matrix.

In \[13\]:

```
cosine_sim = cosine_similarity(count_matrix)
print(cosine_sim)
print(cosine_sim.shape)
```

```
[[1.         0.10540926 0.12038585 ... 0.         0.         0.        ]
 [0.10540926 1.         0.0761387  ... 0.03651484 0.         0.        ]
 [0.12038585 0.0761387  1.         ... 0.         0.11145564 0.        ]
 ...
 [0.         0.03651484 0.         ... 1.         0.         0.04264014]
 [0.         0.         0.11145564 ... 0.         1.         0.        ]
 [0.         0.         0.         ... 0.04264014 0.         1.        ]]
(4803, 4803)
```

## 5) Recommendation[¶](#5)-Recommendation)

In \[14\]:

```
def get_title_from_index(index):
    return df[df.index == index]["title"].values[0]
def get_index_from_title(title):
    return df[df.title == title]["index"].values[0]

movie_user_likes = "Avatar"
movie_index = get_index_from_title(movie_user_likes)
similar_movies = list(enumerate(cosine_sim[movie_index]))

sorted_similar_movies = sorted(similar_movies,key=lambda x:x[1],reverse=True)[1:]

i=0
print(movie_user_likes+"와 비슷한 영화 3편은 "+"\n")
for item in sorted_similar_movies:
    print(get_title_from_index(item[0]))
    i=i+1
    if i==3:
        break
```

```
Avatar와 비슷한 영화 3편은 

Guardians of the Galaxy
Aliens
Star Wars: Clone Wars: Volume 1
```

# Collaborative Filtering[¶](#Collaborative-Filtering)

-   Collaborative Filtering
    -   User base
    -   Item base
    -   Latent Factor Collaborative Filtering -> matrix factorization

`User base` and `Item base` calculate cosine similarity and `Latent Factor` analyzes latent factor using **matrix factorization** !

## (1) User Based Collaborative Filtering[¶](#(1)-User-Based-Collaborative-Filtering)

User Based Collaborative Filtering: a technique used to predict the items that a user might like on the basis of ratings given to that item by the other users who have similar taste with that of the target user.

> "Customers who have similar `taste with you` purchased this product, too!"

## (2) Item-based collaborative filtering[¶](#(2)-Item-based-collaborative-filtering)

Item-based collaborative filtering : the recommendation system to use the similarity between items using the ratings by users.

In general, Item-based collaborative filtering is more accurate.

> "Customers who purchased this `item` also bought this product!"

## (3) Latent Factor Collaborative Filtering[¶](#(3)-Latent-Factor-Collaborative-Filtering)

Latent Factor Collaborative Filtering analyzes latent factor using matrix factorization.

There are some techniques in matrix factorization.

-   SVD(Singular Vector Decomposition)
-   ALS(Alternating Least Squares)
-   NMF(Non-Negative Factorization)

### SVD[¶](#SVD)

SVD is decomposing a matrix `A` to a `M x N` shape matrix.

![image](https://user-images.githubusercontent.com/78291267/154896792-f273850e-82f3-4997-b574-1217c0c70d4e.png)![image](https://user-images.githubusercontent.com/78291267/154896809-3c4974f5-2406-4d1c-b9fe-10e1fedd5076.png)

[특이값 분해(SVD)](https://angeloyeo.github.io/2019/08/01/SVD.html)

[SVD](https://darkpgmr.tistory.com/106)

The reason we use SVD is to **Restore information**.

[numpy.linalg.svd](https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html)

In \[15\]:

```
import numpy as np
from numpy.linalg import svd
```

In \[16\]:

```
np.random.seed(30)
A = np.random.randint(0, 100, size=(4, 4))
A
```

Out\[16\]:

```
array([[37, 37, 45, 45],
       [12, 23,  2, 53],
       [17, 46,  3, 41],
       [ 7, 65, 49, 45]])
```

In \[17\]:

```
svd(A)
```

Out\[17\]:

```
(array([[-0.54937068, -0.2803037 , -0.76767503, -0.1740596 ],
        [-0.3581157 ,  0.69569442, -0.13554741,  0.60777407],
        [-0.41727183,  0.47142296,  0.28991733, -0.72082768],
        [-0.6291496 , -0.46389601,  0.55520257,  0.28411509]]),
 array([142.88131188,  39.87683209,  28.97701433,  14.97002405]),
 array([[-0.25280963, -0.62046326, -0.4025583 , -0.6237463 ],
        [ 0.06881225, -0.07117038, -0.8159854 ,  0.56953268],
        [-0.73215039,  0.61782756, -0.23266002, -0.16767299],
        [-0.62873522, -0.47775436,  0.34348792,  0.50838848]]))
```

As a result we can get matrix U, matirx $\\sum$, matrix V.

In \[18\]:

```
U, Sigma, VT = svd(A)

print('U matrix: {}\n'.format(U.shape),U)
print('Sigma: {}\n'.format(Sigma.shape),Sigma)
print('V Transpose matrix: {}\n'.format(VT.shape),VT)
```

```
U matrix: (4, 4)
 [[-0.54937068 -0.2803037  -0.76767503 -0.1740596 ]
 [-0.3581157   0.69569442 -0.13554741  0.60777407]
 [-0.41727183  0.47142296  0.28991733 -0.72082768]
 [-0.6291496  -0.46389601  0.55520257  0.28411509]]
Sigma: (4,)
 [142.88131188  39.87683209  28.97701433  14.97002405]
V Transpose matrix: (4, 4)
 [[-0.25280963 -0.62046326 -0.4025583  -0.6237463 ]
 [ 0.06881225 -0.07117038 -0.8159854   0.56953268]
 [-0.73215039  0.61782756 -0.23266002 -0.16767299]
 [-0.62873522 -0.47775436  0.34348792  0.50838848]]
```

Now let's restore this again. To restore, we have to do the dot product with U, $\\sum$, V. However, we have to **transform $\\sum$ to a diagonal matrix because its demesion is 1**.

In \[20\]:

```
# searched: np.diag: make or extract from diagonal matrix
Sigma_mat = np.diag(Sigma)
print(Sigma_mat)

A_ = np.dot(np.dot(U, Sigma_mat), VT)
A_
```

```
[[142.88131188   0.           0.           0.        ]
 [  0.          39.87683209   0.           0.        ]
 [  0.           0.          28.97701433   0.        ]
 [  0.           0.           0.          14.97002405]]
```

Out\[20\]:

```
array([[37., 37., 45., 45.],
       [12., 23.,  2., 53.],
       [17., 46.,  3., 41.],
       [ 7., 65., 49., 45.]])
```

### Truncated SVD[¶](#Truncated-SVD)

Truncated SVD = LSA(Latent Semantic analysis)

Truncated SVD decreases the dimension of matrix and decompose it.

[sklearn.decomposition.TruncatedSVD](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html)

[SVD](https://youtu.be/epoHE2rex0g)

### Matrix factorization[¶](#Matrix-factorization)

![image](https://user-images.githubusercontent.com/78291267/154897568-a787383d-9c9a-4205-a077-23b2af9c5507.png)![image](https://user-images.githubusercontent.com/78291267/154897593-ba568663-6f7b-4a81-9766-35a639743680.png)

[matrix factorization](https://youtu.be/ZspR5PZemcs)
