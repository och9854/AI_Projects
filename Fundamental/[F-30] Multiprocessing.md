

# Intro[¶](#Intro)

**Learning Objectives**

-   Understanding Multitasking, parallel programming with python
-   Make Parallel programming with `concurrent.futures` with python

**Contents**

1.  Multitasking

-   Program
-   Profiling
-   scale-up vs scale-out

1.  Multiprocess, Multithread

-   Multithread
-   Multiprocess
-   Thread/Process Pool

1.  Example
    -   `concurrent.futures` module
    -   finding prime number

# Multitasking - What is multitasking?[¶](#Multitasking---What-is-multitasking?)

You need to know how to optimize computing resources.

[Difference Between Concurrency and Parallelism](https://techdifferences.com/difference-between-concurrency-and-parallelism.html)

## Concurrency, Parallelism[¶](#Concurrency,-Parallelism)

-   `Concurrency`: concurrency is about dealing with a lot of things at same time (gives the illusion of simultaneity) or handling concurrent events essentially hiding latency.
-   `Parallelism`: parallelism is about doing a lot of things at the same time for increasing the speed.

[concurrency-vs-parallelism](http://tutorials.jenkov.com/java-concurrency/concurrency-vs-parallelism.html)

## Synchoronous vs Asynchoronous[¶](#Synchoronous-vs-Asynchoronous)

![image](https://user-images.githubusercontent.com/78291267/155264537-56f24134-acc8-4c39-b832-c6223cc0e454.png)

\[[https://poiemaweb.com/js-async](https://poiemaweb.com/js-async)\]

-   Synchronous: In synchronous operations tasks are performed one at a time and only when one is completed, the following is unblocked. In other words, you need to wait for a task to finish to move to the next one.
-   Asynchronous: In asynchronous operations, on the other hand, you can move to another task before the previous one finishes. This way, with asynchronous programming you’re able to deal with multiple requests simultaneously, thus completing more tasks in a much shorter period of time.

# Multitasking - Process, Thread, Profiling[¶](#Multitasking---Process,-Thread,-Profiling)

## Process[¶](#Process)

-   Process: instance of a computer program that is being executed by one or many threads.

You can get the information by `os` module.

In \[1\]:

```
import os

# process ID
print("process ID:", os.getpid())

# user ID
print("user ID:", os.getuid())

# group ID
print("group ID:", os.getgid())

# 현재 작업중인 디렉토리
print("current Directory:", os.getcwd())
```

```
process ID: 13
user ID: 0
group ID: 0
current Directory: /aiffel/aiffel/Fundamental
```

## Profiling[¶](#Profiling)

In \[9\]:

```
import timeit
        
def f1():
    s = set(range(100))

    
def f2():
    l = list(range(100))

    
def f3():
    t = tuple(range(100))


def f4():
    s = str(range(100))

    
def f5():
    s = set()
    for i in range(100):
        s.add(i)

def f6():
    l = []
    for i in range(100):
        l.append(i)
    
def f7():
    s_comp = {i for i in range(100)}

    
def f8():
    l_comp = [i for i in range(100)]
    

if __name__ == "__main__":
    t1 = timeit.Timer("f1()", "from __main__ import f1")
    t2 = timeit.Timer("f2()", "from __main__ import f2")
    t3 = timeit.Timer("f3()", "from __main__ import f3")
    t4 = timeit.Timer("f4()", "from __main__ import f4")
    t5 = timeit.Timer("f5()", "from __main__ import f5")
    t6 = timeit.Timer("f6()", "from __main__ import f6")
    t7 = timeit.Timer("f7()", "from __main__ import f7")
    t8 = timeit.Timer("f8()", "from __main__ import f8")
    print("set               :", t1.timeit(), '[ms]')
    print("list              :", t2.timeit(), '[ms]')
    print("tuple             :", t3.timeit(), '[ms]')
    print("string            :", t4.timeit(), '[ms]')
    print("set_add           :", t5.timeit(), '[ms]')
    print("list_append       :", t6.timeit(), '[ms]')
    print("set_comprehension :", t5.timeit(), '[ms]')
    print("list_comprehension:", t6.timeit(), '[ms]')
```

```
set               : 1.67967756500002 [ms]
list              : 0.7644742299999052 [ms]
tuple             : 0.7476368370007549 [ms]
string            : 0.40529437700024573 [ms]
set_add           : 5.749327528999856 [ms]
list_append       : 5.083722508001301 [ms]
set_comprehension : 5.817576972000097 [ms]
list_comprehension: 5.059891418000916 [ms]
```

[파이썬 프로파일러 - cProfile, profile](https://docs.python.org/ko/3/library/profile.html)

[line profiler를 사용하여 파이썬의 각 라인이 어떻게 돌아가는지를 알아보자.](https://frhyme.github.io/python-libs/python_line_profileing_in_python/)

## Profiling[¶](#Profiling)

-   Profiling: a form of dynamic program analysis that measures, for example, the space (memory) or time complexity of a program, the usage of particular instructions, or the frequency and duration of function calls.

# Multitasking - Scale up vs Scale out[¶](#Multitasking---Scale-up-vs-Scale-out)

![image](https://user-images.githubusercontent.com/78291267/155361666-8af41d12-192a-43ba-b52a-f34418f4f96a.png)

# Multithread[¶](#Multithread)

## (1) Making Thread[¶](#(1)-Making-Thread)

-   Import thread
-   Class inheritance

In \[11\]:

```
from threading import *

class Delivery(Thread):
	def run(self):
		print("delivery")

class RetriveDish(Thread):
	def run(self):
		print("Retriving Dish")

work1 = Delivery()
work2 = RetriveDish()

def main():
	work1.run()
	work2.run()

if __name__ == '__main__':
    main()
```

```
delivery
Retriving Dish
```

-   Checking thread

In \[14\]:

```
# checking thread
from threading import *

class Delivery:
    def run(self):
        print("delivering")

work1 = Delivery()
print(work1.run)

class Delivery(Thread):
    def run(self):
        print("delivering")

work2 = Delivery()
print(work2.run)
```

```
<bound method Delivery.run of <__main__.Delivery object at 0x7f7a0c2d22b0>>
<bound method Delivery.run of <Delivery(Thread-12, initial)>>
```

## (2) Using Thread[¶](#(2)-Using-Thread)

We can instantiate and make thread instead of class inheritance.

For instantiation, we need `target` and `args`.

In \[15\]:

```
from threading import *
from time import sleep

Stopped = False

def worker(work, sleep_sec):    # 일꾼 스레드입니다.
    while not Stopped:          # 그만 하라고 할때까지
        print('do ', work)      # 시키는 일을 하고
        sleep(sleep_sec)        # 잠깐 쉽니다.
    print('retired..')          # 언젠가 이 굴레를 벗어나면, 은퇴할 때가 오겠지요?
        
t = Thread(target=worker, args=('Overwork', 3))    # 일꾼 스레드를 하나 생성합니다. 열심히 일하고 3초간 쉽니다.
t.start()    # 일꾼, 이제 일을 해야지? 😈
```

```
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
do  Overwork
```

In \[16\]:

```
# 이 코드 블럭을 실행하기 전까지는 일꾼 스레드는 종료하지 않습니다. 
Stopped = True    # 일꾼 일 그만하라고 세팅해 줍시다. 
t.join()          # 일꾼 스레드가 종료할때까지 기다립니다. 
print('worker is gone.')
```

```
retired..
worker is gone.
```

# Using Multiprocess in python[¶](#Using-Multiprocess-in-python)

[multiprocessing - Process-based parallelism - Python 3.7.12 documentation](https://docs.python.org/3.7/library/multiprocessing.html)

## Making Procecss[¶](#Making-Procecss)

In \[17\]:

```
import multiprocessing as mp

def delivery():
    print('delivering...')

p = mp.Process(target=delivery, args=())
p.start()
```

```
delivering...
```

## Using process[¶](#Using-process)

Process has methods like `start()`, `join()`, `terminate()`

**Process**

```
p = mp.Process(target=delivery, args=())
p.start() # 프로세스 시작
p.join() # 실제 종료까지 기다림 (필요시에만 사용)
p.terminate() # 프로세스 종료
```

# Using Thread, Process pool in python[¶](#Using-Thread,-Process-pool-in-python)

There are two ways to make pool.

-   Using [Queue](https://docs.python.org/3.7/library/queue.html)
-   Using `ThreadPoolExecutor` in [concurrent.futures](https://docs.python.org/ko/3.7/library/concurrent.futures.html) library

## Concurrent.futures module[¶](#Concurrent.futures-module)

There are four class.

-   Executor
-   `ThreadPoolExecutor`
-   `ProcessPoolExecutor`
-   `Future`

### `ThreadPoolExecutor`[¶](#ThreadPoolExecutor)

If we use `Executor` class, we can make readable codes with `with` context administrator when we make, start and join thread.

```
with ThreadPoolExecutor() as executor:
    future = executor.submit(function, args)
```

In \[19\]:

```
from concurrent.futures import ThreadPoolExecutor

class Delivery:
    def run(self):
        print("delivering")
w = Delivery()

with ThreadPoolExecutor() as executor:
    future = executor.submit(w.run)
```

```
delivering
```

### `MultiProcessing.Pool`[¶](#MultiProcessing.Pool)

In \[20\]:

```
from multiprocessing import Pool
from os import getpid

def double(i):
    print("I'm processing ", getpid())    # pool 안에서 이 메소드가 실행될 때 pid를 확인해 봅시다.
    return i * 2

with Pool() as pool:
      result = pool.map(double, [1, 2, 3, 4, 5])
      print(result)
```

```
I'm processing I'm processing I'm processing  I'm processing   9896
99I'm processing 
 98

 97
[2, 4, 6, 8, 10]
```

we can see that `double(i)` method was executed as mutilprocess on differenet pids throught pool.

# Practice[¶](#Practice)

[concurrent.futures - Launching parallel tasks - Python 3.7.9 documentation](https://docs.python.org/ko/3.7/library/concurrent.futures.html)

Let's deal with those two parts.

-   `map()` function in `Executor` class
-   `ProcessPoolExecutor`

-   Look in the codes

```
import math
import concurrent

PRIMES = [
    112272535095293,
    112582705942171,
    112272535095293,
    115280095190773,
    115797848077099,
    1099726899285419]

def is_prime(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False

    sqrt_n = int(math.floor(math.sqrt(n)))
    for i in range(3, sqrt_n + 1, 2):
        if n % i == 0:
            return False
    return True

def main():
    with concurrent.futures.ProcessPoolExecutor() as executor:
        for number, prime in zip(PRIMES, executor.map(is_prime, PRIMES)):
            print('%d is prime: %s' % (number, prime))

if __name__ == '__main__':
    main()
```

### 1) Problem[¶](#1)-Problem)

-   determine the number is prime or not

In \[22\]:

```
import math
import concurrent

PRIMES = [
    112272535095293,
    112582705942171,
    112272535095293,
    115280095190773,
    115797848077099,
    1099726899285419]

print("*    .\n·   *\n  *   *\n🌲 🦕 🌳")
```

```
*    .
·   *
  *   *
🌲 🦕 🌳
```

### 2) `is_prime`[¶](#2)-is_prime)

In \[23\]:

```
# searched: math.floor
def is_prime(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False

    sqrt_n = int(math.floor(math.sqrt(n))) # throws away the decimal
    for i in range(3, sqrt_n + 1, 2):
        if n % i == 0:
            return False
    return True
print("🌲      🦕...")
```

```
🌲      🦕...
```

### 3) call the main function[¶](#3)-call-the-main-function)

-   Run `map()` function in `executor` ran in `ProcessPoolExecutor()`.
-   Used `with` to be ran in `concurrent.futures` library process pool.

> Map-reduce style?  
> Map-reduce architecture: a programming model used for efficient processing in parallel over large data-sets in a distributed manner

In \[26\]:

```
import time

def main():
    print("병렬처리 시작")
    start = time.time()
    with concurrent.futures.ProcessPoolExecutor() as executor:
        for number, prime in zip(PRIMES, executor.map(is_prime, PRIMES)):
            print('%d is prime: %s' % (number, prime))
    end = time.time()
    print("병렬처리 수행 시각", end-start, 's')
    
    print("단일처리 시작")
    start = time.time()
    for number, prime in zip(PRIMES, map(is_prime, PRIMES)):
        print('%d is prime: %s' % (number, prime))
    end = time.time()
    print("단일처리 수행 시각", end-start, 's')
print(" ❣\n🌲🦕.......")
```

```
 ❣
🌲🦕.......
```

### 4) Full code[¶](#4)-Full-code)

In \[28\]:

```
main()
```

```
병렬처리 시작
112272535095293 is prime: True
112582705942171 is prime: True
112272535095293 is prime: True
115280095190773 is prime: True
115797848077099 is prime: True
1099726899285419 is prime: False
병렬처리 수행 시각 1.978498935699463 s
단일처리 시작
112272535095293 is prime: True
112582705942171 is prime: True
112272535095293 is prime: True
115280095190773 is prime: True
115797848077099 is prime: True
1099726899285419 is prime: False
단일처리 수행 시각 2.6864123344421387 s
```

In \[ \]:
