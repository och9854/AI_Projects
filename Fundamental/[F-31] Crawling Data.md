
## Intro[¶](#Intro)

**학습 목표**

-   HTML 문서의 개념에 대해서 이해한다.
-   태그의 형식에 대해서 이해한다.
-   크롤링을 위한 패키지인 BeautifulSoup4의 사용법을 이해한다.
-   머신 러닝 분류 방법인 나이브 베이즈 분류기의 사용법을 익힌다.

준비물

1.  Making Directory

```
$ mkdir -p ~/aiffel/news_crawler
```

1.  Installing Mecab

오늘 실습에서는 Mecab이라는 형태소 분석기를 사용합니다. 혹시 Mecab이 깔려있지 않은 환경이라면 터미널을 열고 아래의 커맨드로 Mecab을 설치해줍니다. 중간에 암호 입력이 필요할 수 있습니다.

```
$ git clone https://github.com/SOMJANG/Mecab-ko-for-Google-Colab.git
$ cd Mecab-ko-for-Google-Colab
$ bash install_mecab-ko_on_colab190912.sh
```

1.  installing pip package

```
$ pip install beautifulsoup4
$ pip install newspaper3k
$ pip install konlpy
```

# HTML, Tag[¶](#HTML,-Tag)

-   Crawling: the process of indexing data on web pages by using a program or automated script.These automated scripts or programs are known by multiple names, including web crawler, spider, spider bot, and often shortened to crawler.

# Selector[¶](#Selector)

-   Selector: a pattern of elements and other terms that tell the browser which HTML elements should be selected to have the CSS property values inside the rule applied to them

Selector makes us to facilitate crawling.

```
<html> 
    <head> 
    </head> 
    <body> 
        <h1> 장바구니
            <p id='clothes' class='name' title='라운드티'> 라운드티
                <span class = 'number'> 25 </span> 
                <span class = 'price'> 29000 </span> 
                <span class = 'menu'> 의류</span> 
                <a href = 'http://www.naver.com'> 바로가기 </a> 
            </p> 
            <p id='watch' class='name' title='시계'> 시계
                <span class = 'number'> 28 </span>
                <span class = 'price'> 32000 </span> 
                <span class = 'menu'> 악세서리 </span> 
                <a href = 'http://www.facebook.com'> 바로가기 </a> 
            </p> 
        </h1> 
    </body> 
</html>
```

# BeautifulSoup package[¶](#BeautifulSoup-package)

# HTML, Tag[¶](#HTML,-Tag)

---

-   Crawling: the process of indexing data on web pages by using a program or automated script.These automated scripts or programs are known by multiple names, including web crawler, spider, spider bot, and often shortened to crawler.

# Selector[¶](#Selector)

---

-   Selector: a pattern of elements and other terms that tell the browser which HTML elements should be selected to have the CSS property values inside the rule applied to them

Selector makes us to facilitate crawling.

```
<html> 
    <head> 
    </head> 
    <body> 
        <h1> 장바구니
            <p id='clothes' class='name' title='라운드티'> 라운드티
                <span class = 'number'> 25 </span> 
                <span class = 'price'> 29000 </span> 
                <span class = 'menu'> 의류</span> 
                <a href = 'http://www.naver.com'> 바로가기 </a> 
            </p> 
            <p id='watch' class='name' title='시계'> 시계
                <span class = 'number'> 28 </span>
                <span class = 'price'> 32000 </span> 
                <span class = 'menu'> 악세서리 </span> 
                <a href = 'http://www.facebook.com'> 바로가기 </a> 
            </p> 
        </h1> 
    </body> 
</html>
```

# BeautifulSoup package[¶](#BeautifulSoup-package)

---

If we use `BeautifulSoup`, we can easily extract what we want from some tags. This package allows us to extract data from `HTML`, `XML` document.

[BeautifulSoup 공식 문서](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)

## How to use[¶](#How-to-use)

---

In \[30\]:

```
from bs4 import BeautifulSoup

#- HTML 문서를 문자열 html로 저장합니다.
html = '''
<html> 
    <head> 
    </head> 
    <body> 
        <h1> 장바구니
            <p id='clothes' class='name' title='라운드티'> 라운드티
                <span class = 'number'> 25 </span> 
                <span class = 'price'> 29000 </span> 
                <span class = 'menu'> 의류</span> 
                <a href = 'http://www.naver.com'> 바로가기 </a> 
            </p> 
            <p id='watch' class='name' title='시계'> 시계
                <span class = 'number'> 28 </span>
                <span class = 'price'> 32000 </span> 
                <span class = 'menu'> 악세서리 </span> 
                <a href = 'http://www.facebook.com'> 바로가기 </a> 
            </p> 
        </h1> 
    </body> 
</html>
'''

#- BeautifulSoup 인스턴스를 생성합니다.
#- 두번째 매개변수는 분석할 분석기(parser)의 종류입니다.
soup = BeautifulSoup(html, 'html.parser')
```

After making instance, we get information using `select()` method. Now the name of instance is `soup` so we get information with `soup.select()`.

[soup?](https://m.blog.naver.com/kiddwannabe/221177292446)

[Extract information using BeautifulSoup](https://m.blog.naver.com/kiddwannabe/221177292446)

In \[31\]:

```
# practices 

print(soup.select('body'))
print("--------------------------------------------------")
print(soup.select('p'))

print("--------------------------------------------------")
print(soup.select('h1 .name .menu')) # In h1 -> .name -> .menu
```

```
[<body>
<h1> 장바구니
            <p class="name" id="clothes" title="라운드티"> 라운드티
                <span class="number"> 25 </span>
<span class="price"> 29000 </span>
<span class="menu"> 의류</span>
<a href="http://www.naver.com"> 바로가기 </a>
</p>gt;
<p class="name" id="watch" title="시계"> 시계
                <span class="number"> 28 </span>
<span class="price"> 32000 </span>
<span class="menu"> 악세서리 </span>
<a href="http://www.facebook.com"> 바로가기 </a>
</p>
</h1>
</body>]
--------------------------------------------------
[<p class="name" id="clothes" title="라운드티"> 라운드티
                <span class="number"> 25 </span>
<span class="price"> 29000 </span>
<span class="menu"> 의류</span>
<a href="http://www.naver.com"> 바로가기 </a>
</p>, <p class="name" id="watch" title="시계"> 시계
                <span class="number"> 28 </span>
<span class="price"> 32000 </span>
<span class="menu"> 악세서리 </span>
<a href="http://www.facebook.com"> 바로가기 </a>
</p>]
--------------------------------------------------
[<span class="menu"> 의류</span>, <span class="menu"> 악세서리 </span>]
```

# newspaper3k package[¶](#newspaper3k-package)

---

`newspaper3k` package is made for crawling news data. If user inputs URL of newspaper, it outputs the `title, text` of newspaper.

We are going to crawl [this site](https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=101&oid=030&aid=0002881076) so let's take a look of tags and selectors.

In \[32\]:

```
from newspaper import Article

#- 파싱할 뉴스 기사 주소입니다.
url = 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=101&oid=030&aid=0002881076'

#- 언어가 한국어이므로 language='ko'로 설정해줍니다.
article = Article(url, language='ko')
article.download()
article.parse()
```

In \[33\]:

```
#- title
print('기사 제목 :')
print(article.title)
print('')

#- texts
print('기사 내용 :')
print(article.text)
```

```
기사 제목 :
[AI 사피엔스 시대]자연어처리 기술, 컴퓨팅 파워 경쟁 시대로

기사 내용 :
[Copyright ⓒ 전자신문 & 전자신문인터넷, 무단전재 및 재배포 금지]

주로 아이디어와 기술력으로 경쟁했던 자연어처리 인공지능(AI) 분야는 점차 컴퓨팅 파워 싸움으로 무게 추가 이동하고 있다. 모델이 대형화되면서 향상된 퍼포먼스 확보에 필요한 자금 규모도 커지고 있다. 자칫 대기업 자본력에 휘둘릴 수 있다는 우려도 함께 나온다.자연어처리(NLP)는 인간이 사용하는 언어 체계를 기계가 인식하도록 알고리즘을 디자인하는 기술이다. 흔히 말하는 컴퓨터 혹은 인간과 대화하는 컴퓨터 관련 기술이 포함된다.목적에 따라 세 가지 카테고리로 나뉜다. 인간이 제기한 질문에 자동으로 적절한 답을 찾아주는 '질의응답(QA)', 원하는 업무를 지시했을 때 작업을 수행하는 '테스크 컴플리션', 그리고 특별한 목적이 없는 대화를 의미하는 '오픈도메인 컨버세이션(비목적성 대화)'이 있다. 각기 발전해왔던 세 가지 기술은 지난 2018년 구글의 인공지능 언어모델 '버트(BERT)'의 등장으로 패러다임이 전환됐다. 압도적인 성능으로 대량의 프리트레이닝(사전학습)이 가능해지면서 굳이 셋을 구분할 필요가 없어진 것이다.기계학습 연구에서 모델을 학습할 때는 지도학습과 비지도학습, 강화학습 중 하나를 골라 활용한다. 지도학습은 사람이 적절한 입력과 출력을 부여하는 방식이다. 정답이 정해져 있고 기계의 정답률도 쉽게 측정할 수 있다. 반면에 비지도학습은 정답이 정해지지 않은 데이터에 대해서도 기계가 스스로 클러스터링 등을 통해 학습한다. 체계화되지 않은 대량의 데이터를 학습 가능하지만 학습이 맞게 됐는지 확인하기 어렵다.버트는 기존 AI 학습 방법을 혁신적으로 바꿔놨다는 평가를 받는다. 자연어처리를 교사 없이 양방향으로 사전 학습하는 최초의 시스템이다. 비지도학습 방식을 사용하면서도 기존 존재했던 어떤 기술보다 뛰어난 성능을 보여준다. 최근 1년 반 동안 버트를 필두로 AI 모델은 급격히 대형화되는 추세다.이는 기존의 빅데이터 개념을 훨씬 상회하는 초 대규모 데이터를 프리트레이닝 단계에서 학습 가능하게 만드는 결과로 이어졌다. 성능도 비약적으로 상승했다. 가장 최근 구글이 선보인 제너레이션 모델은 제시된 문장에 대해 인간의 80~90% 수준으로 답변 문장을 만들어 낸다.문제는 모델이 대형화되면서 기술을 구성하는 요소 중 알고리즘 대비 대량의 데이터셋과 컴퓨팅 파워 중요성이 더 커졌다는 점이다. 버트 자체는 '파인튜닝' 방식으로 다양한 분야에 확대 적용이 가능하지만 버트 성능에 준하는 최신 대형 모델을 만들어내려면 현존 최신 텐서처리장치(TPU)로도 학습에 수십 일이 걸린다.국내 한 AI 스타트업 대표는 “국내 상황은 데이터셋·인력·자금 인프라 모두 중국이나 영어권에 비해 떨어지는 게 사실이다. 자연어처리 모델 하나 만드는 데 10억~20억원씩 비용이 들어 고민이 많다”면서 “지금 글로벌 시장에서 격차가 나기 시작하면 이후에는 따라잡기 어려워진다. 정부 차원의 관심과 지원이 필요하다”고 말했다.이형두기자 dudu@etnews.com
```

# Naver News Crawling[¶](#Naver-News-Crawling)

---

## (1) Understanding News URL, page[¶](#(1)-Understanding-News-URL,-page)

---

The purpose of this is categorizing news.

![image](https://user-images.githubusercontent.com/78291267/155717741-1e65278d-219d-41e9-8460-192a8c76a1ed.png)

If you click `View page Source`, you can see some structures.

![image](https://user-images.githubusercontent.com/78291267/155717854-c2e5e544-1735-4c01-98c3-3cfed65ff726.png)

## (2) Making Crawler using BeautifulSoup and newspaper3k[¶](#(2)-Making-Crawler-using-BeautifulSoup-and-newspaper3k)

---

`make_urllist` returns URL list using BeautifulSoup. It gets `page_num`, `code`, `date` as inputs.

In \[34\]:

```
# 크롤러를 만들기 전 필요한 도구들을 임포트합니다.
import requests
import pandas as pd
from bs4 import BeautifulSoup

# 페이지 수, 카테고리, 날짜를 입력값으로 받습니다.
def make_urllist(page_num, code, date): 
  urllist= []
  for i in range(1, page_num + 1):
    url = 'https://news.naver.com/main/list.nhn?mode=LSD&mid=sec&sid1='+str(code)+'&date='+str(date)+'&page='+str(i)
    headers = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.90 Safari/537.36'}
    news = requests.get(url, headers=headers)

    # BeautifulSoup의 인스턴스 생성합니다. 파서는 html.parser를 사용합니다.
    soup = BeautifulSoup(news.content, 'html.parser')

    # CASE 1
    news_list = soup.select('.newsflash_body .type06_headline li dl')
    # CASE 2
    news_list.extend(soup.select('.newsflash_body .type06 li dl'))
        
    # 각 뉴스로부터 a 태그인 <a href ='주소'> 에서 '주소'만을 가져옵니다.
    for line in news_list:
        urllist.append(line.a.get('href'))
  return urllist
```

In \[35\]:

```
# total fourty URL lists

url_list = make_urllist(2, 101, 20200506)
print('뉴스 기사의 개수: ',len(url_list))
```

```
뉴스 기사의 개수:  40
```

In \[36\]:

```
# check five URL
url_list[:5]
```

Out\[36\]:

```
['https://news.naver.com/main/read.naver?mode=LSD&mid=sec&sid1=101&oid=057&aid=0001451723',
 'https://news.naver.com/main/read.naver?mode=LSD&mid=sec&sid1=101&oid=057&aid=0001451721',
 'https://news.naver.com/main/read.naver?mode=LSD&mid=sec&sid1=101&oid=057&aid=0001451718',
 'https://news.naver.com/main/read.naver?mode=LSD&mid=sec&sid1=101&oid=003&aid=0009849190',
 'https://news.naver.com/main/read.naver?mode=LSD&mid=sec&sid1=101&oid=057&aid=0001451717']
```

In \[37\]:

```
# making code dictionary
idx2word = {'101' : '경제', '102' : '사회', '103' : '생활/문화', '105' : 'IT/과학'}
```

In \[38\]:

```
from newspaper import Article

#- 데이터프레임을 생성하는 함수입니다.
def make_data(urllist, code):
  text_list = []
  for url in urllist:
    article = Article(url, language='ko')
    article.download()
    article.parse()
    text_list.append(article.text)

  #- 데이터프레임의 'news' 키 아래 파싱한 텍스트를 밸류로 붙여줍니다.
  df = pd.DataFrame({'news': text_list})

  #- 데이터프레임의 'code' 키 아래 한글 카테고리명을 붙여줍니다.
  df['code'] = idx2word[str(code)]
  return df
```

In \[39\]:

```
data = make_data(url_list, 101) # 경제 카테고리
#- 상위 10개만 출력해봅니다.
data[:10]
```

Out\[39\]:

|   | news | code |
| --- | --- | --- |
| 0 | 고려은단이 5월을 맞아 응원 메시지를 공유하는 ‘5월 5글자로 응원 부탁해!’ 이벤... | 경제 |
| 1 | 코리아나화장품의 민감성 피부를 위한 저자극 스킨케어 브랜드 '프리엔제'가 마르고 건... | 경제 |
| 2 | 서울장수주식회사가 부드럽고 달콤한 맛으로 인기를 모으고 있는 생막걸리 ‘인생막걸리’... | 경제 |
| 3 | \[서울=뉴시스\] 오동현 기자 = 모바일 게임 기업 컴투스는 3D 모바일 야구 게임 ... | 경제 |
| 4 | 대원제약이 2020년 상반기 신입과 경력 정기 공채를 실시합니다.정기 공채 모집분야... | 경제 |
| 5 | \[AFP=연합뉴스\] \[AFP=연합뉴스\]\\n\\n"요즘은 잔인한 날"…리프트도 앞서 9... | 경제 |
| 6 | 이재용 삼성전자 부회장이 6일 삼성전자 서울 서초사옥에서 대국민 사과 회견을 하기 ... | 경제 |
| 7 | JW중외제약이 A형 혈우병 예방요법제 ‘헴리브라피하주사를 출시하고 본격적인 마케팅 ... | 경제 |
| 8 | 옵티팜과 휴벳바이오가 공동 개발중인 백신 후보 물질에 대해 마우스, 기니피그, 미니... | 경제 |
| 9 | \[한국경제TV 신동호 기자\]\\n\\n전남 나주시와 충북 청주시가 방사광 가속기 구축사... | 경제 |

## (3) Collecting data[¶](#(3)-Collecting-data)

---

Let's collect other codes, too.

In \[40\]:

```
code_list = [102, 103, 105]

code_list
```

Out\[40\]:

```
[102, 103, 105]
```

`make_total_data()` function calls `make_urllist` and `make_data` functions.

In \[42\]:

```
from multiprocessing import Pool
import random
import time, os

def make_total_data(page_num, code_list, date):
  start = int(time.time())  
  num_cores = 4  
  df = None
  for code in code_list:
    pool = Pool(num_cores)
    url_list = make_urllist(page_num, code, date)
    df_temp = make_data(url_list, code)
    print(str(code)+'번 코드에 대한 데이터를 만들었습니다.')
    pool.close()
    pool.join()
    time.sleep(random.randint(0,1))
    if df is not None:
      df = pd.concat([df, df_temp])
    else:
      df = df_temp

  print("***run time(sec) :", int(time.time()) - start)
  return df
```

In \[43\]:

```
df = make_total_data(1, code_list, 20200506)
```

```
102번 코드에 대한 데이터를 만들었습니다.
103번 코드에 대한 데이터를 만들었습니다.
105번 코드에 대한 데이터를 만들었습니다.
***run time(sec) : 43
```

In \[44\]:

```
# page * news * code_list = 1 * 20 * 3 = 60

print('뉴스 기사의 개수: ',len(df))
```

```
뉴스 기사의 개수:  60
```

In \[45\]:

```
#check
df.sample(10)
```

Out\[45\]:

|   | news | code |
| --- | --- | --- |
| 9 | 경찰 로고./뉴스1 © News1 신채린 기자 경찰 로고./뉴스1 © News1 신... | 사회 |
| 18 | 기사 섹션 분류 안내\\n\\n기사의 섹션 정보는 해당 언론사의 분류를 따르고 있습니다... | IT/과학 |
| 7 | 동영상 뉴스\\n\\n5월 날씨가 맞나 싶으시죠.오늘도 초여름이었습니다.심지어 올 들어... | 생활/문화 |
| 17 | 국내 최초로 건립되는 노동문학관이 6일 첫 삽을 떴다.‘노동문학관건립위원회(위원장 ... | 사회 |
| 13 | 블랙홀을 품은 삼중성계 HR 6819 상상도 \[ESO/L. Calcada 제공/ 재... | IT/과학 |
| 12 | 동영상 뉴스\\n\\n\[앵커\]다음 주 고등학교 3학년부터 순차적인 등교수업이 시작되는데... | 사회 |
| 17 | 바티칸의 사도궁 집무실에서 6일(현지시간) 인터넷 중계 방식의 수요 일반 알현 훈화... | 생활/문화 |
| 19 | 이남식 서울예대 총장(왼쪽) 안심키트 배포. 사진제공=서울예대 이남식 서울예대 총장... | 사회 |
| 5 | 질서정연 코로나19 확산 방지를 위한 ‘물리적 거리 두기’가 ‘생활 속 거리 두기’... | 사회 |
| 18 | 기사 섹션 분류 안내\\n\\n기사의 섹션 정보는 해당 언론사의 분류를 따르고 있습니다... | 사회 |

### Crawling[¶](#Crawling)

---

Let's make a model which classifies categories.

In \[47\]:

```
# 아래 주석처리된 코드의 주석을 해제하고 실행을 하면 대량 크롤링이 진행됩니다. 
# 위에서 수행했던 크롤링의 10배 분량이 수행될 것입니다. 한꺼번에 너무 많은 크롤링 요청이 서버에 전달되지 않도록 주의해 주세요. 
# 기사 일자를 바꿔보면서 데이터를 모으면 더욱 다양한 데이터를 얻을 수 있게 됩니다. 

df = make_total_data(10, code_list, 20220224)
```

```
102번 코드에 대한 데이터를 만들었습니다.
103번 코드에 대한 데이터를 만들었습니다.
105번 코드에 대한 데이터를 만들었습니다.
***run time(sec) : 412
```

In \[48\]:

```
import os

# 데이터프레임 파일을 csv 파일로 저장합니다.
# 저장경로는 이번 프로젝트를 위해 만든 폴더로 지정해 주세요.
csv_path = os.getenv("HOME") + "/aiffel/news_crawler/news_data.csv"
df.to_csv(csv_path, index=False)

if os.path.exists(csv_path):
  print('{} File Saved!'.format(csv_path))
```

```
/aiffel/aiffel/news_crawler/news_data.csv File Saved!
```

## (4) Data preprocessing[¶](#(4)-Data-preprocessing)

---

We need to preprocess data and transform to number so that run in computer.

### Data Preprocessing[¶](#Data-Preprocessing)

---

Read CSV file again with this code below.

In \[49\]:

```
csv_path = os.getenv("HOME") + "/aiffel/news_crawler/news_data.csv"
df = pd.read_table(csv_path, sep=',')
df.head()
```

Out\[49\]:

|   | news | code |
| --- | --- | --- |
| 0 | ▲특별승급 영예를 안은 황규호 주무관. ⓒ창원시 ▲특별승급 영예를 안은 황규호 주무... | 사회 |
| 1 | \[KBS 강릉\]동해시 북평중학교와 봉오마을을 연결하는 간선도로가 개설됩니다.이를 위... | 사회 |
| 2 | \[KBS 강릉\]강원도소방본부는 오늘(24일) 고성군 토성면 일대에서 산림 화재 대응... | 사회 |
| 3 | \[KBS 강릉\]민주노총 강원지역본부는 오늘(24일) 고용노동부 강릉지청 앞에서 기자... | 사회 |
| 4 | \[KBS 강릉\]접경지역의 경제 활성화를 위해 국군복지단의 각종 사업 입찰에서 접경지... | 사회 |

We have to remove number, english, HTML tags using regex.

In \[50\]:

```
# 정규 표현식을 이용해서 한글 외의 문자는 전부 제거합니다.
df['news'] = df['news'].str.replace("[^ㄱ-ㅎㅏ-ㅣ가-힣 ]","")
df['news']
```

```
/tmp/ipykernel_14/3649439000.py:2: FutureWarning: The default value of regex will change from True to False in a future version.
  df['news'] = df['news'].str.replace("[^ㄱ-ㅎㅏ-ㅣ가-힣 ]","")
```

Out\[50\]:

```
0      특별승급 영예를 안은 황규호 주무관 창원시 특별승급 영예를 안은 황규호 주무관 창원...
1       강릉동해시 북평중학교와 봉오마을을 연결하는 간선도로가 개설됩니다이를 위해 동해시는...
2       강릉강원도소방본부는 오늘일 고성군 토성면 일대에서 산림 화재 대응 훈련을 진행했습...
3       강릉민주노총 강원지역본부는 오늘일 고용노동부 강릉지청 앞에서 기자회견을 갖고 최근...
4       강릉접경지역의 경제 활성화를 위해 국군복지단의 각종 사업 입찰에서 접경지 기업에 ...
                             ...                        
595    파이낸셜뉴스 셀트리온은 일 코로나 변이에 대응해 흡입형 칵테일 항체 치료제 진단키트...
596    기사 섹션 분류 안내기사의 섹션 정보는 해당 언론사의 분류를 따르고 있습니다 언론사...
597    모두에게 보여주고 싶은 기사라면 이 기사를 추천합니다 버튼을 눌러주세요 집계 기간 ...
598    디지털데일리 신제인 기자 코로나 이후 비대면 서비스 수요가 증가하면서 산업 전반에서...
599    이번 연구성과가 게재된 국제학술지 어드밴스드 사이언스 월 후면 표지 제공 이번 연구...
Name: news, Length: 600, dtype: object
```

In \[51\]:

```
# check null
print(df.isnull().sum())
```

```
news    0
code    0
dtype: int64
```

In \[53\]:

```
# remove rebundancy

df.drop_duplicates(subset=['news'], inplace=True)

print('뉴스 기사의 개수: ',len(df))
```

```
뉴스 기사의 개수:  511
```

### Explore data[¶](#Explore-data)

Take a look about distribution of each samples.

In \[54\]:

```
import matplotlib.pyplot as plt
plt.rcParams["font.family"] = "NanumGothic"

df['code'].value_counts().plot(kind = 'bar')
```

Out\[54\]:

```
<AxesSubplot:>
```

![](data:image/png;base64,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)

In \[55\]:

```
# count each categories
print(df.groupby('code').size().reset_index(name = 'count'))
```

```
    code  count
0  IT/과학    168
1     사회    168
2  생활/문화    175
```

### Tokenize[¶](#Tokenize)

In general, strings should be divided by some standards. we call these standards `token`.

There are some tokenizers for Korean, such as [KoNLPy](https://konlpy-ko.readthedocs.io/ko/v0.4.3/), [kakao/khaiii](https://github.com/kakao/khaiii), [Mecab](https://bitbucket.org/eunjeon/mecab-ko-dic/src/master/).

We will use `Mecab` today.

In \[56\]:

```
from konlpy.tag import Mecab
tokenizer = Mecab()

kor_text = '밤에 귀가하던 여성에게 범죄를 시도한 대 남성이 구속됐다서울 제주경찰서는 \
            상해 혐의로 씨를 구속해 수사하고 있다고 일 밝혔다씨는 지난달 일 피해 여성을 \
            인근 지하철 역에서부터 따라가 폭행을 시도하려다가 도망간 혐의를 받는다피해 \
            여성이 저항하자 놀란 씨는 도망갔으며 신고를 받고 주변을 수색하던 경찰에 \
            체포됐다피해 여성은 이 과정에서 경미한 부상을 입은 것으로 전해졌다'

#- 형태소 분석, 즉 토큰화(tokenization)를 합니다.
print(tokenizer.morphs(kor_text))
```

```
['밤', '에', '귀가', '하', '던', '여성', '에게', '범죄', '를', '시도', '한', '대', '남성', '이', '구속', '됐', '다', '서울', '제주', '경찰서', '는', '상해', '혐의', '로', '씨', '를', '구속', '해', '수사', '하', '고', '있', '다고', '일', '밝혔', '다', '씨', '는', '지난달', '일', '피해', '여성', '을', '인근', '지하철', '역', '에서부터', '따라가', '폭행', '을', '시도', '하', '려다가', '도망간', '혐의', '를', '받', '는다', '피해', '여성', '이', '저항', '하', '자', '놀란', '씨', '는', '도망갔으며', '신고', '를', '받', '고', '주변', '을', '수색', '하', '던', '경찰', '에', '체포', '됐', '다', '피해', '여성', '은', '이', '과정', '에서', '경미', '한', '부상', '을', '입', '은', '것', '으로', '전해졌', '다']
```

### Remove Stopwords[¶](#Remove-Stopwords)

-   `Stopwords`: words which does not add much meaning to a sentence.

In \[57\]:

```
stopwords = ['에','는','은','을','했','에게','있','이','의','하','한','다','과','때문','할','수','무단','따른','및','금지','전재','경향신문','기자','는데','가','등','들','파이낸셜','저작','등','뉴스']
```

In \[58\]:

```
# 토큰화 및 토큰화 과정에서 불용어를 제거하는 함수입니다.
def preprocessing(data):
  text_data = []

  for sentence in data:
    temp_data = []
    #- 토큰화
    temp_data = tokenizer.morphs(sentence) 
    #- 불용어 제거
    temp_data = [word for word in temp_data if not word in stopwords] 
    text_data.append(temp_data)

  text_data = list(map(' '.join, text_data))

  return text_data
```

In \[59\]:

```
text_data = preprocessing(df['news'])
print(text_data[0])
```

```
특별 승급 영예 를 안 황규호 주무관 창원시 특별 승급 영예 를 안 황규호 주무관 창원시 허성무 창원 시장 일 특별 승급 심사 위원회 를 개최 해 년 도 최고 업무 실적 우수 자 로 년 묵 생계 대책 민원 해결 해양 정책 황규호 주무관 특별 승급 결정 다년간 표류 던 회성동 창원 자족 형 복합 행정 타운 조성 본 궤도 올린 신 도시 조성 유태재 주무관 성과 상여금 배 지급 대상자 으로 선정 년 부산항 신항 건설 로 어업권 소멸 된 어업 인 생계 대책 요구 년 어업 피해 보상 약정 체결 토지 처분 위한 협의 년 부터 지속 됐으나 토지 매각 시점 기준 대한 이견 커 그동안 협상 결렬 돼 왔었 지난해 월 허 시장 긴급 제안 으로 국민 권익 위원회 민원 중재 요청 으로 방향 선회 고 시 권익 위 를 수차례 방문 해 직접 중재 게 만드 커다란 성과 를 이끌어냈 과정 속 에서 황 주무관 주요 쟁점 해소 초안 직접 작성 여 개월 간 개별 협의 와 통합 회의 를 거쳐 끊임없 건의 와 설득 반복 끝 최적 합의 안 확정 지난해 월 생계 대책 부지 처분 계획 승인 받 고 소유 권 이전 완료 함 으로써 년 끌어온 소멸 어업 인 민원 종결 선언 창 원자 족형 복합 행정 타운 조성 년 개발제한구역 해제 이후 공공 기관 유치 와 사업 성 문제 로 경남 개발 공사 에서 사업 포기 함 따라 도시 개발 구역 지정 개발 계획 수립 고시 를 지 못해 년 넘 게 표류 해 왔 유 주무관 년 국토 교통부 낙동강 유역 환경청 관련 부서 협의 와 도시 계획 위원회 심의 중요 절차 를 이행 고 그해 월 개발 계획 고시 까지 이끌 어 냈 공공 기관 유치 도 적극 나 서 창 원지방 법원 마산 지원 창 원지방 검찰청 마산 지청 이전 협의 를 완료 특별 승급 제도 지방 공무원 보수 규정 근거 해 시행 되 고 해당 실국 소장 확인 후 부서장 추천 받 아 신청 한다 사실 조사 를 거쳐 특별 승급 대상자 로 타당 한지 여부 를 검토 고 직원 평가 다면 평가 도 참고 여 특별 승급 심사 위원회 에서 특별 승급 여부 를 최종 결정 한다 올해 특별 승급 신청자 총 명 으로 사실 조사 를 통해 해양 정책 황규호 주무관 신 도시 조성 유태재 주무관 명 으로 압축 해 일 특별 승급 최종 심사 이루 어 졌
```

# Using Machine Learning[¶](#Using-Machine-Learning)

---

Let's apply machine learning models. We will use `Naive Bayes`.

[Naive Bayes'](https://www.youtube.com/watch?v=3JWLIV3NaoQ&ab_channel=MinsukHeo%ED%97%88%EB%AF%BC%EC%84%9D)

In \[60\]:

```
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.feature_extraction.text import TfidfTransformer
from sklearn.naive_bayes import MultinomialNB
from sklearn import metrics
```

In \[61\]:

```
#- 훈련 데이터와 테스트 데이터를 분리합니다.
X_train, X_test, y_train, y_test = train_test_split(text_data, df['code'], random_state = 0)
```

In \[62\]:

```
print('훈련용 뉴스 기사의 개수 :', len(X_train))
print('테스트용 뉴스 기사의 개수 : ', len(X_test))
print('훈련용 레이블의 개수 : ', len(y_train))
print('테스트용 레이블의 개수 : ', len(y_test))
```

```
훈련용 뉴스 기사의 개수 : 383
테스트용 뉴스 기사의 개수 :  128
훈련용 레이블의 개수 :  383
테스트용 레이블의 개수 :  128
```

[Tf-IDF](https://youtu.be/meEchvkdB1U)

Let's change each news to TF-IDF vector and train model.

`CountVectorizer.fit_transform()` learns word data and tranforms document data to document-form matrix!

[CountVectorizer.fit\_transform](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html#sklearn.feature_extraction.text.CountVectorizer.fit_transform)

In \[63\]:

```
#- 단어의 수를 카운트하는 사이킷런의 카운트벡터라이저입니다.
count_vect = CountVectorizer()
X_train_counts = count_vect.fit_transform(X_train)

#- 카운트벡터라이저의 결과로부터 TF-IDF 결과를 얻습니다.
tfidf_transformer = TfidfTransformer()
X_train_tfidf = tfidf_transformer.fit_transform(X_train_counts)

#- 나이브 베이즈 분류기를 수행합니다.
#- X_train은 TF-IDF 벡터, y_train은 레이블입니다.
clf = MultinomialNB().fit(X_train_tfidf, y_train)
```

In \[64\]:

```
# Change to TF-IDF vector when got texts

def tfidf_vectorizer(data):
  data_counts = count_vect.transform(data)
  data_tfidf = tfidf_transformer.transform(data_counts)
  return data_tfidf
```

In \[65\]:

```
#test
new_sent = preprocessing(["민주당 일각에서 법사위의 체계·자구 심사 기능을 없애야 한다는 \
                           주장이 나오는 데 대해 “체계·자구 심사가 법안 지연의 수단으로 \
                          쓰이는 것은 바람직하지 않다”면서도 “국회를 통과하는 법안 중 위헌\
                          법률이 1년에 10건 넘게 나온다. 그런데 체계·자구 심사까지 없애면 매우 위험하다”고 반박했다."])
print(clf.predict(tfidf_vectorizer(new_sent)))
```

```
['사회']
```

In \[66\]:

```
#test
new_sent = preprocessing(["인도 로맨틱 코미디 영화 <까립까립 싱글>(2017)을 봤을 때 나는 두 눈을 의심했다. \
                          저 사람이 남자 주인공이라고? 노안에 가까운 이목구비와 기름때로 뭉친 파마머리와, \
                          대충 툭툭 던지는 말투 등 전혀 로맨틱하지 않은 외모였다. 반감이 일면서 \
                          ‘난 외모지상주의자가 아니다’라고 자부했던 나에 대해 회의가 들었다.\
                           티브이를 꺼버릴까? 다른 걸 볼까? 그런데, 이상하다. 왜 이렇게 매력 있지? 개구리와\
                            같이 툭 불거진 눈망울 안에는 어떤 인도 배우에게서도 느끼지 못한 \
                            부드러움과 선량함, 무엇보다 슬픔이 있었다. 2시간 뒤 영화가 끝나고 나는 완전히 이 배우에게 빠졌다"])
print(clf.predict(tfidf_vectorizer(new_sent)))
```

```
['생활/문화']
```

In \[67\]:

```
# test
new_sent = preprocessing(["20분기 연속으로 적자에 시달리는 LG전자가 브랜드 이름부터 성능, 디자인까지 대대적인 변화를 \
                          적용한 LG 벨벳은 등장 전부터 온라인 커뮤니티를 뜨겁게 달궜다. 사용자들은 “디자인이 예쁘다”, \
                          “슬림하다”는 반응을 보이며 LG 벨벳에 대한 기대감을 드러냈다."])
print(clf.predict(tfidf_vectorizer(new_sent)))
```

```
['IT/과학']
```

In \[68\]:

```
# real
y_pred = clf.predict(tfidf_vectorizer(X_test))
print(metrics.classification_report(y_test, y_pred))
```

```
              precision    recall  f1-score   support

       IT/과학       0.70      0.91      0.79        44
          사회       0.85      0.87      0.86        38
       생활/문화       0.97      0.67      0.79        46

    accuracy                           0.81       128
   macro avg       0.84      0.82      0.81       128
weighted avg       0.84      0.81      0.81       128

```
