# 탐색 알고리즘1: 순차 탐색 (Sequential Search)

### 1. 순차 탐색 (Sequential Search) 이란?
* 탐색은 여러 데이터 중에서 원하는 데이터를 찾아내는 것을 의미
* 데이터가 담겨있는 리스트를 앞에서부터 하나씩 비교해서 원하는 데이터를 찾는 방법

<div class="alert alert-block alert-warning">
<strong><font color="blue" size="4em">프로그래밍 연습</font></strong><br>
임의 리스트가 다음과 같이 rand_data_list로 있을 때, 원하는 데이터의 위치를 리턴하는 순차탐색 알고리즘 작성해보기<br>
- 가장 기본적인 방법이므로, 직접 작성해보겠습니다.
- 원하는 데이터가 리스트에 없을 경우 -1을 리턴
</div>
<pre>
# 데이터 준비: data_list 10개 만들기
from random import *
 
rand_data_list = list()
for num in range(10):
    rand_data_list.append(randint(1, 100))
</pre>

```
from random import *

rand_data_list = list()
for num in range(10):
    rand_data_list.append(randint(1, 100))
```
```
rand_data_list
```
[71, 63, 75, 33, 6, 37, 81, 79, 3, 29]

```
def sequencial(data_list, search_data):
    for index in range(len(data_list)):
        if data_list[index] == search_data:
            return index
    return -1
```
```
sequencial(rand_data_list, 4)
```
-`

### 2. 알고리즘 분석
* 최악의 경우 리스트 길이가 n일 때, n번 비교해야 함
  - O(n)
