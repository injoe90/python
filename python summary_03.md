# 리스트와 튜플

> 1. 리스트
> 2. 튜플



## 1. 리스트

```python
score2 = [[1,2,3,4],[5,6,7,8],[9,10,11,12]];
allsum = 0
allcnt = 0
for i1 in score2:
    sum = 0
    cnt = len(i1)
    for i2 in i1:
        sum += i2;
    print('%d %.2f' % (sum, sum/cnt));
    allsum += sum;
    allcnt += cnt;
print('%d %.2f' % (allsum,allsum/allcnt));
```

	* 리스트는 여러 개의 값을 집합적으로 저장한다. 다른 언어의 배열에 해당하며 실제로 배열과 비슷한 방식으로 사용한다.
 * 리스트 자체도 요소가 될 수 있다.
   	* 이중 리스트를 순회하며 최종값을 모두 읽으려면 루프도 중복으로 돌아야 한다.  

```python
s = [];
s.append(20);
s.append(30);
s.append(10);
s.append(40);
s.append(50);
s.insert(2,99);
s[3] = [1,2,3];

# 실행결과
[20, 30, 99, [1, 2, 3], 40, 50];
print('-------------------------')
del s[0];
s.remove(50);

# 실행결과
[30, 99, [1, 2, 3], 40];
```

 * append는 인수로 전달한 요소를 리스트 끝에 덧붙여 추가한다. insert는 삽입할 위치와 요소 값을 받아 리스트의 중간에 삽입한다.
   	* 삽입과 달리 리스트를 대입하면, 해당 요소 값은 바뀌며 요소 개수는 변하지 않는다.
   	* 리스트에 다른 리스트를 추가하여 병합할 때에는 extend 메서드를 사용한다.
 * remove는 인수로 받은 요소 값을 찾아 삭제한다. 
   	* 해당 값이 없으면 예외를 발생시키고 값이 2개 이상이면 처음에 발견한 요소만 제거한다.
    * del 명령은 순서 값을 지정하여 삭제한다.
      	* del(리스트 이름[n]), del 리스트 이름[n]
	* 일정한 범위에 속한 여러 요소를 지울 때에는 해당 범위에 대한 빈 리스트를 대입한다.

```python
s = [30, 99, [1, 2, 3], 40];
print(s.pop());
print(s.pop(1));
print(s.index(30));
s.append(30);
print(s.count(30));
print(s);

# 실행결과
40
99
0
2
[30, [1, 2, 3], 30]
```

	* pop은 삭제한 요소를 꺼내 출력한다. 첨자를 통해 대상을 지정하지만 인수가 없으면 마지막 요소를 선택한다.
	* index는 특정 요소의 위치를 찾으며 발견되지 않을 경우에 예외를 발생시킨다.
	* count는 특정 요소 값의 개수를 조사한다.

```python
print('start');
cart = [];

def viewCart(c):
    ptotal = 0;
    ctotal = 0;
    for item in c:
        print('Item: %s %d %d' % (item[0], int(item[1]), int(item[2])));
        ptotal += int(item[1]);
        ctotal += int(item[2]);
    print('Total: %d %d' % (ptotal, ptotal / ctotal));

while True:
    menu = input('Input menu(i,v,r,q)');
    if menu == 'i':
        item = input('Input Item(name,price,count)');
        item = item.split(' ');
        cart.append(item);
    if menu == 'v':
        viewCart(cart);
    if menu == 'r':
        name = input('Input Item(name)');
        for i in cart:
            if name in i:
                cart.remove(i);
                print(i[0]+'이 삭제되었습니다.');
    if menu == 'q':
        print('bye');
        break;
print('end');
```



## 2. 튜플

```python
score = (10,20,30,40);
print(score[0]);
print(score[:2]);
scorelist = list(score);
print(scorelist);
score = score + (50,60,70);
print(score);

# 실행결과
10
(10, 20)
[10, 20, 30, 40]
(10, 20, 30, 40, 50, 60, 70)
```

 * 튜플은 리스트와 유사하게 요소 타입에 제한을 두지 않지만, 이를 다시 편집할 수 없기 때문에 상수 리스트라고 한다.
   	* 리스트에 비해 튜플에서 처리속도가 빠르다.
   	* 콤마로 구분된 값이 나열되어 있으면 파이썬은 이를 튜플이라고 파악한다.
   	* 요소가 하나만 있는 튜플은 일반 변수와 구분되지 않아 값 다음에 여분의 콤마를 찍어 튜플임을 표시한다.
 * 튜플의 요소를 읽거나 튜플끼리 연결하거나 * 연산자로 튜플의 요소를 반복할 수 있다. 그러나 튜플의 요소를 변경하거나 삭제하는 것은 불가능하다.
   	* 리스트가 제공하는 append나 insert와 같은 메서드가 없어 요소를 추가/삽입/삭제할 수 없다.
   	* 튜플이 제공하는 메서드에는 index와 count만 있다.
	* 좌변의 변수 개수와 튜플의 요소 개수가 같을 때 튜플의 요소가 하나씩 변수에 대입된다.
 * 리스트와 튜플은 내부 구조가 비슷해서 상호 변환이 가능하다.
   	* 튜플을 리스트로 변환하면 요소에 대해 편집을 할 수 있다.



# 사전과 집합

> 1. 사전
> 2. 집합
> 3. 컬렉션 관리



## 1. 사전

```python
item2 = {'name':'item1','price':1000,'count':1};
print(item2['price']);
print(item2.get('pri','Empty'));

if 'count' in item2:
    print(item2.get('count') * 100);

item2['date'] = '20210105';
print(item2);
del (item2['date']);
print(item2);

# 실행결과
1000
Empty
100
{'name': 'item1', 'price': 1000, 'count': 1, 'date': '20210105'}
{'name': 'item1', 'price': 1000, 'count': 1}
```

 * 사전(Dictinary)은 키와 값의 쌍을 저장하는 대용량의 자료구조이다. 해시 알고리즘을 사용하여 일대일로 대응되는 특성이 있어 맵 또는 연관 배열이라고도 부른다.
   	* 사전을 정의할 때에는 {} 안에 키 : 값 형태로 나열한다.
   	* 키는 값을 찾는 기준이어서 중복되지 않아야 하지만 값은 그렇지 않다.
   	* 키는 읽기 전용이므로 변경할 수 없으므로 리스트와 달리 튜플은 키로 쓸 수 있다.
    * 사전은 빠른 검색을 위해 키에서 저장하는 위치를 결정하며 최대한 찾기 쉬운 위치에 저장하므로 순서를 유지하지 않는다. 검색이 엄청나게 빨라 거의 실시간으로 찾아낸다.
      	* 찾는 키가 없으면 예외가 발생하므로 예외 처리 구문을 사용해야 하며 get 메서드로 값을 찾을 수 있다. 이 경우에 get은 None을 반환하고 두 번째 인수를 통해 None 대신 다른 값을 지정할 수 있다.
 * 사전은 변경할 수 있는 자료형이므로 실행 중에 삽입, 삭제, 수정 등의 편집을 할 수 있다.
   	* [] 괄호와 대입문을 주로 사용한다.
   	* 키가 이미 존재하면 기존 값이 변경되고 없으면 새로 삽입된다.
	* 없는 키에 대해 del 명령을 사용하면 예외가 발생하고 사전의 모든 요소를 삭제하여 완전히 비울 떄에는 clear 메서드를 호출한다.

```python
st = {'name':'james', 'major':'it', 'age':20};

ks = st.keys();
print(ks); # List

for i in ks:
    print(i,sep=' ');

vs = st.values(); # List
print(vs);

for i in vs:
    print(i,sep=' ');

item = st.items(); # List + Tuple
print(item);

for i in item:
    print(i,sep=' ');

# 실행결과
dict_keys(['name', 'major', 'age'])
name
major
age
dict_values(['james', 'it', 20])
james
it
20
dict_items([('name', 'james'), ('major', 'it'), ('age', 20)])
('name', 'james')
('major', 'it')
('age', 20)
```

 * 사전의 키, 값 목록을 얻으려면 keys, values 메서드를 호출한다.
   	* keys 메서드는 키의 목록만 들어있는 리스트 객체를 반환한다.
   	* values 메서드는 값의 목록만 들어있는 리스트 객체를 반환한다.
   	* items 메서드는 키와 값의 쌍을 튜플로 묶은 객체를 반환한다.
   	* 그러나 이 메서드로 출력한 것은 진짜 리스트가 아니므로 append, insert 등의 편집 함수를 호출할 수 없다.

```python
for sts in st:
    print('%s 학생의 평균 %.2f' % (sts.get('id'),
                              (sts.get('ko')+sts.get('en')+sts.get('ma'))/3));

kosum = 0;
ensum = 0;
masum = 0;

for sts in st:
    kosum += sts.get('ko');
    ensum += sts.get('en');
    masum += sts.get('ma');
cnt = len(st)
print('과목별 평균 %.2f %.2f %.2f' % (kosum/cnt,ensum/cnt,masum/cnt));

# 실행결과
st1 학생의 평균 96.33
st2 학생의 평균 93.00
st3 학생의 평균 93.33
과목별 평균 91.00 93.67 98.00
```



```python
data = """Python is an interpreted, high-level and general-purpose programming language. Python's design philosophy emphasizes code readability with its notable use of significant whitespace. Its language constructs and object-oriented approach aim to help programmers write clear, logical code for small and large-scale projects.[27]
Python is dynamically typed and garbage-collected. It supports multiple programming paradigms, including structured (particularly, procedural), object-oriented, and functional programming. Python is often described as a "batteries included" language due to its comprehensive standard library.[28]
Python was created in the late 1980s, and first released in 1991, by Guido van Rossum as a successor to the ABC programming language. Python 2.0, released in 2000, introduced new features, such as list comprehensions, and a garbage collection system with reference counting, and was discontinued with version 2.7 in 2020.[29] Python 3.0, released in 2008, was a major revision of the language that is not completely backward-compatible and much Python 2 code does not run unmodified on Python 3. With Python 2's end-of-life, only Python 3.6.x[30] and later are supported, with older versions still supporting e.g. Windows 7 (and old installers not restricted to 64-bit Windows).
Python interpreters are supported for mainstream operating systems and available for a few more (and in the past supported many more). A global community of programmers develops and maintains CPython, a free and open-source[31] reference implementation. A non-profit organization, the Python Software Foundation, manages and directs resources for Python and CPython development.
As of December 2020 Python ranked third in TIOBE’s index of most popular programming languages, behind C and Java."""

count = {}; # 'a':10,'b':120
for c in data:
    if c.isalpha() == False:
        continue;
    c = c.lower();
    if c not in count:
        count[c] = 1; # {'p':1,'y':1}
    else:
        count[c] += 1;

print(count.items());
result = sorted(count.items(),key=lambda x:x[1],reverse=True);
print(result[0:5]);

# 실행결과
dict_items([('p', 64), ('y', 31), ('t', 107), ('h', 42), ('o', 111), ('n', 120), ('i', 95), ('s', 89), ('a', 123), ('e', 146), ('r', 99), ('d', 71), ('g', 44), ('l', 60), ('v', 10), ('u', 39), ('m', 44), ('z', 2), ('c', 51), ('b', 19), ('w', 18), ('f', 24), ('j', 5), ('k', 2), ('x', 2)])
[('e', 146), ('a', 123), ('n', 120), ('o', 111), ('t', 107)]
```



## 2. 집합

```python
data = {11,2,31,14,5};
data.remove(2);
data.add(10);
data.add(12);
print(data);

data2 = set(li);
data2.add(1);
print(data2);

data3 = list(data2);
data3.append(1);
print(data3);

# 실행결과
{5, 10, 11, 12, 14, 31}
{1, 2, 3, 4}
[1, 2, 3, 4, 1]
```

	* 집합은 {} 괄호 안에 키를 콤마로 구분하여 나열한다.
 * 키의 중복은 허락하지 않으며 순서도 별 의미가 없다.
   	* set 함수는 빈 집합을 만들기도 하고 다른 자료형을 집합형으로 변환하기도 한다.
   	* 순서가 중요하지 않으므로 원본 단어에 있던 순서와 달라진다.
    * 집합은 수정할 수 있다.
      	* 원소를 추가할 때에는 add 메서드, 제거할 때에는 remove 메서드, 합집합을 만들 때에는 update 메서드를 이용한다.

|     연산      | 기호 |        메서드        |
| :-----------: | :--: | :------------------: |
|    합집합     |  \|  |        union         |
|    교집합     |  &   |     intersection     |
|    차집합     |  -   |      difference      |
| 배타적 차집합 |  ^   | symmetric_difference |
|   부분집합    |  <=  |       issubset       |
|  진 부분집합  |  <   |                      |
|   포함집합    |  >=  |      issuperset      |
|  진 포함집합  |  >   |                      |



## 3. 컬렉션 관리 함수

```python
score = [90,80,60,100];
score.sort(reverse=True); # score 안에 있는 자료 간 순서만 재배치
print(score);
score2 = sorted(score,reverse=True); # 새로운 리스트를 생성
print(score2);

for i in enumerate(score2,1): # 1(k)번부터 시작
    print(i);
    print(type(i));

# 실행결과
[100, 90, 80, 60]
[100, 90, 80, 60]
(1, 100)
<class 'tuple'>
(2, 90)
<class 'tuple'>
(3, 80)
<class 'tuple'>
(4, 60)
<class 'tuple'>
```

 * enumerate 함수는 순서값과 요소 값을 한꺼번에 구한다.
   	* 리스트의 순서 값과 요소 갑을 튜플로 묶은 자료형을 출력한다.

```python
def myfilter2(n):
    return n >= 80;

score = [90,80,60,100];
for i in filter(myfilter2,score):
    print(i);

print('------------------------------------');

for i in filter(lambda x:x>=90, score): # 필터 함수를 1줄로 요약
    print(i);
# 실행결과
90
80
100
------------------------------------
90
100
```

	* filter 함수는 리스트의 요소 중 조건에 맞는 것만 골라낸다. filter 함수가 리스트를 반환하므로 for문을 이용하면 조건에 맞는 요소를 모두 구할 수 있다.
	* 모든 요소에 대해 함수를 호출하여 제어권을 넘겨주므로 정밀한 점검을 할 수 있어 자유도가 높다.

```python
def mymap(n):
    return n / 3;
score = [93,87,65,100];

for i in map(mymap, score): # 실수로 자동 전환, 리스트의 모든 자료에 적용
    print(i);
print('-------------------------');
for i in map(lambda x:x/3, score):
    print(i);
--------------------------------------
# 실행결과
31.0
29.0
21.666666666666668
33.333333333333336
-------------------------
31.0
29.0
21.666666666666668
33.333333333333336
```

	* map 함수는 모든 요소에 대해 변환 함수를 호출하여 새 요고 값으로 구성된 리스트를 생성한다.
	* 인수 구조는 filter 함수와 동일하며 원본인 리스트는 읽기만 할 뿐 변경하지 않는다.
 * 람다는 이름이 없고 입력과 출력만으로 함수를 정의하는 방법이다.
   	* 'lambda 인수: 식, 적용대상'으로 나타내고 인수와 반환할 값을 나타낸다.

```python
a = 1;
b = a;
print('a:%d b:%d' % (a,b));

a = 5;
print('a:%d b:%d' % (a,b));

list1 = [1,2,3,4,];
list2 = list1; # 주소 설정
list3 = list1.copy(); # 변수의 경우처럼 값을 복사

print(list1);
print(list2);

print('_________________________');
list1[0] = 100;
print(list1);
print(list2);
print(list3);

# 실행결과
a:1 b:1
a:5 b:1
[1, 2, 3, 4]
[1, 2, 3, 4]
_________________________
[100, 2, 3, 4]
[100, 2, 3, 4]
[1, 2, 3, 4]
```

 * 메모리에는 변수만 입력되며 컬렉션은 주소만 설정된다.
   	* 따라서 참조 리스트의 내용이 바뀌면 이 리스트에 대한 주소를 지닌 여러 개의 컬렉션이 영향을 받는다.



# 표준 모듈과 예외 처리

> 1. import문
> 2. 시간
> 3. 난수
> 4. 예외 처리



## 1. import문

```python
# import 문서 이름 as 약어
# from random import randint; 
# import random; random;
```

	* 특정 함수를 지정하지 않고 단순히 외부 모듈을 모두 호출하면 해당 패키지의 모든 함수를 사용하므로 처리 속도가 늦어질 수 있다.



## 2. 시간

```python
import time;
import datetime;

t = time.time();
print(time.time()); #1970.1.1 ~ 현재까지의 초
localtime = time.localtime();
print(localtime);

localtime = datetime.datetime.now();
print(type(localtime));
print(localtime);

print(localtime.year);
print(localtime.month);
print(localtime.day);

# 실행결과
1610469875.341965
time.struct_time(tm_year=2021, tm_mon=1, tm_mday=13, tm_hour=1, tm_min=44, tm_sec=35, tm_wday=2, tm_yday=13, tm_isdst=0)
<class 'datetime.datetime'>
2021-01-13 01:44:35.341965
2021
1
13
```

	* time.time 함수는 시간을 하나의 수치로 간단히 표현할 수 있어 계산이나 저장이 간편하고 다른 시스템과 통신에도 유리하다.
	* time 모듈 대신 datetime 모듈의 now 함수 또는 today 함수를 통해 현재 지역 시간을 쉽게 구할 수 있다. 

```python
import time;

start = time.time();
for i in range(1,100): # 로그인을 하는 부분
    print(i);
    time.sleep(0.05);
end = time.time();

print(end - start);
print('----------------------')
import time;

i = 1;
while True:
    print('Try Connection..');
    if i == 10:
        print('Connected..');
        break;
    time.sleep(3);
    print('Retry..');
    i += 1;
```

	* time.sleep 함수는 CPU를 지정한 시간만큼 잠재워 아무 것도 하지 않고 시간을 끈다.

```python
import calendar;
calendar.setfirstweekday(6) # 첫 간의 요일을 바꾸는 함수이다. 기본 값은 월요일이다.
print(calendar.calendar(2020));

print(calendar.weekday(2020,12,25)); # 특정 날짜가 무슨 요일인지 조사하는 함수이다.
```

## 3. 난수

```python
import random;

for i in range(3):
    print(random.random());

for i in range(3):
    print(random.randint(1,10));

for i in range(3):
    print(random.uniform(1,100));
print('----------------------------');

f = ['a','b','c','d'];
print(random.choice(f));

print(f);
random.shuffle(f);
print(f);
print(random.sample(f,2));
print(random.sample(range(1,46),6));

# 실행결과
0.15289032934184932
0.15478466044452321
0.8751496221741618
4
9
1
53.51134162565857
24.927352139538947
87.45407465764589
----------------------------
a
['a', 'b', 'c', 'd']
['c', 'b', 'a', 'd']
['c', 'a']
[41, 11, 43, 5, 37, 6]
```



## 4. 예외 처리

```python
import time;

try:
    print('네트워크 접속 시도');
    time.sleep(2);
    print('네트워크 접속 성공');
    time.sleep(2);
    8/0;
    print('네트워크 데이터 전송');
    time.sleep(2);
except:
    print('문제 발생');
finally:
    print('네트워크 접속 해지');
    
# 실행결과
네트워크 접속 시도
네트워크 접속 성공
문제 발생
네트워크 접속 해지
```

 * 에러가 발생하면 즉시 프로그램이 종료되고 이후의 명령을 무시한다.
   	* 될 수 있으면 문제를 해결하고 정 안되면 어떤 문제가 있는지 친절하게 알려야 한다.
 * try 블록의 코드를 실행하다가 예외가 발생하면 except 블록으로 도약한다.
   	* 예외별로 except 블록을 여러 개 작성하면 경우에 따라 적절한 블록으로 도약한다.
   	* except 블록이 아무리 많아도 먼저 발생한 예외에 맞는 하나만 선택한다.
   	* 두 개 이상의 예외를 하나의 except 블록에서 동시에 받을 수 있다. 이 경우에 예외를 괄호로 묶어 튜플로 지정한다. 즉, except (ValueError, IndexError):
 * raise 명령은 고의적으로 예외를 발생시킨다. 치명적인 문제가 발생했을 때 호출원으로 예외를 던져 작업이 잘못되었음을 알린다.
   	* 상황에 맞는 예외를 직접 정의할 수 있고 try ~ except 구문으로 이를 처리해야 한다.
   	* 예외 코드를 사용하지 않으면 수정할 부분이 많아지고 외부인의 관리보수가 힘들어진다.
   	* 네트워크 프로그래밍에서는 계속 접속된 상태로 인식되지 않도록 반드시 해지를 해야 하는데, 이 과정에서 항상 예외를 고려해야 한다.
 * finally 블록은 예외 발생 벼우와 상관없이 반드시 실행해야 할 명령을 지정한다.
   	* 네트워크 프로그래밍에서는 항상 finally 블록을 사용하여 접속을 끊어야 한다.
   	* 외부 자원을 사용하는 모든 코드에서도 해제를 신경써야 한다.



