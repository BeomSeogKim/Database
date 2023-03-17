# 기본 Python 문법

* [네이밍 컨벤션](#네이밍-컨벤션)
* [타입 힌트](#타입-힌트)
* [리스트 컴프리헨션](#리스트-컴프리헨션)



### 네이밍 컨벤션

> 파이썬의 Naming Convention은 자바와 달리 각 단어를 밑줄(_)로 구분하여 표기하는 Snake Case를 사용한다.

[참고]

* Snake Case 

  * 뱀과 같은 모양에서 유래.  각 단어를 언더스코어(_)로 구분한다. 

  * 일반적으로 모두 소문자를 사용하지만 가끔 시작 문자는 대문자를 사용하기도 한다. 

  * 활용 예시: tommy_blog_name: str = "tom"

* Camel Case

  * 낙타와 같은 모양에서 유래. 각 단어를 대소문자로 구분하여 작명한다. 
  * 단어의 첫 문자는 대문자로 작성 
    * 다만 첫단어의 시작은 소문자로 작성
  * 활용 예시: tommyBlogName="tom"

* Pascal Case

  * CamelCase 규칙을 따르지만 첫 단어의 시작도 대문자로 작성
  * 활용 예시: TommyBlogName="tom"



### 타입 힌트

> 변수 혹은 함수의 리턴 타입들을 명시해주는 방식

[example]

```python
def solution(s):
  ...
```

이렇게 작성하는 경우 빠르게 타이핑을 할 수 있다는 장점이 있지만 변수 s는 무엇인지, solution의 return 값은 무엇인지 알기 어렵다는 단점이 있다. 

[example-type hint]

```python
def solution(s: str) -> str:
  ...
```

위와 같이 변수 s, 그리고 함수의 return 값들을 명시해주는 방식을 type hint 라고 한다. 

위의 코드를 보게되면 바로 s는 문자이며 함수의 return 값도 문자임을 바로 알 수 잇다. 





### 리스트 컴프리헨션

> 파이썬은 map, filter와 같은 함수형 기능을 지원하며 람다 표현식도 지원한다.

예를들어 주어진 값에 대해 10을 더해 리스트를 만들어야 한다고 가정하자.

for문을 반복할 경우 다음과 같이 만들 수 있다. 

```python
number = [1,2,3]
a = []
for n in range(len(number)):
  a.append(n + 10)
```



이러한 코드를 python에서는 다음과 같이 작성할 수 있다.

```python
a = list(map(lambda x : x + 10, [1,2,3]))
```

