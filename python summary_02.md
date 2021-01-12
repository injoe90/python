## 함수

> 1. 함수
> 2. 인수
> 3. 변수의 범위



```python
def calcsum(end):
    sum = 0;
    for d in range(int(end)+1):
        sum += d;
    return sum;

result = calcsum(100);
print(result);

# 실행결과
5050
```

	* 반복해서 사용하는 코드는 한번 정의하고 나서 이를 계속 사용하는 것이 좋다.
 * 함수명은 명칭이므로 자유롭게 하되 동작을 잘 표현하는 동사로 붙이는 것이 좋다.
   	* 함수 이름은 무조건 다르게 해야 하거나 새로운 파일에 각각 작성해야 한다.
 * 인수 목록은 호출원이 함수로 전달하는 작업거리이며 함수 내부에서 사용한다.
   	* 호출문에서 어떤 인수를 전달하는가에 따라 함수의 동작과 리턴 값이 달라진다.
      	* 함수에 return이 없으면 None이 출력된다.
      	* 함수에서 출력을 하는 경우가 많으므로 함수 내에서는 print 함수를 될 수 있으면 사용하지 않는다.

```python
def f4(*n):
    sum = 0;
    for d in n:
        sum +=d;
    return sum;

def f5(m,*n):
    sum = 0;
    for d in n:
        sum +=d;
    return sum +m;

import p140
result4 = p140.f4(1,2,3,4,5);
print(result4);

result5 = p140.f5(100,1,2,3,4,5);
print(result5);

# 실행결과
15
115
```

 * 일반적인 함수는 정의문에 필요한 인수이ㅡ 개수가 명시되어 있고 호출할 때 이 숫자에 맞게 실인수를 넘겨야 한다.
   	* 가변 인수는 임의 개수의 인수를 받으며 인수 이름 앞에 '*' 기호를 붙인다.
 * 가변 인수는 이후의 모든 인수를 다 포함하므로 인수 목록의 마지막에 와야 한다.
   	* 가변 인수 앞에는 일반 인수가 올 수 있지만 뒤쪽에는 올 수 없다.
      	* 가변 인수가 2개 이상 있어서도 안 된다.

```python
def f6(begin=1, end=2, step=1):
    """ bgien:star data ... data default value = 1"""
    sum = 0;
    for d in range(begin, end+1, step):
        sum += d;
    return sum;

def f7(**args):
    b = args['b'];
    e = args['e'];
    s = args['s'];
    sum = 0;
    for d in range(b, e + 1, s):
        sum += d;
    return sum;

def f8(n,*m,**a):
    print('TEST');
    print(n);
    print(m);
    print(a);

import p140
result6 = p140.f6(step=2, end=100,begin=1);
print(result6);

result7 = p140.f7(s=2,e=100,b=1);
print(result7);

p140.f8('datas',1,2,3,4,5,start=10,end=100);

# 실행결과
2500
2500
TEST
datas
(1, 2, 3, 4, 5)
{'start': 10, 'end': 100}
```

 * 인수가 많으면 호출원에서 작업 지시를 섬세하게 전달할 수 있어 활용성이 높아진다. 이때, 잘 바뀌지 않는 인수는 목록에서 기본 값을 지정한다.
   	* 기본 값이 있는 인수는 일종의 옵션으로 취급하여 생략할 수 있고 이 경우에 기본 값을 전달한 것으로 가정한다.
      	* 따라서 남이 만든 함수에서 기본 값이 어떻게 설정되었는가를 확인해야 한다.
 * 인수의 수가 많으면 헷갈리기 쉽다. 순서와 무관하게 인수 이름을 지정하여 대입 형식으로 전달하는 방식을 핵심어 인수(Keyword Argument)라고 한다.
   	* 생략한 인수에는 기본 값이 적용된다.
      	* 위치 인수보다 호출 구문이 길고 인수의 이름을 정확하게 외워야 한다는 단점이 있다.
 * 핵심어 인수를 가변으로 취할 때에는 해당 인수 목록에 '**' 기호를 붙인다.
    * 일반 인수, 위치 인수, 핵심어 인수 순으로 온다.

```python
def calcsum(n):
    sum = 0;
    for num in range(n+1):
        sum +- num;
    return sum;

price = 1000;
def sale():
	global price;
    price = 500;
                     
sale();
print(price);
# 실행결과
500
```

 * 함수 내부에서 선언하는 변수를 지역 변수라고 한다.
   	* 지역 변수는 함수 안에서만 사용될 뿐 밖으로는 알려지지 않는다. 따라서 함수가 실행 중인 동안에만 존재한다.
      	* 함수 내부에서 처음 대입하는 변수는 항상 지역 변수로 간주한다.
     	* 변수 이름 간 충돌을 피하고 함수 동작에 필요한 모든 것을 내부에 포함시켜 독립성을 향상시킬 수 있다. 따라서 다른 함수의 지역 변수와 이름 충돌을 걱정할 필요가 없다.
   	* 지역 변수는 함수 동작에 필요한 모든 것을 내부에 완벽하게 소유할 수 있도록 하여 재활용성을 향상시킨다.
 * 함수 바깥에서 선언하는 변수를 전역 변수라고 한다.
   	* 전역 변수는 프로그램 소속이어서 어디에서나 참조할 수 있다.
    * 파이썬은 명시적인 선언이 없고 대입에 의해 변수를 생성하므로 초기화하는 장소에 따라 범위가 결정된다.
      	* 지역, 전역끼리는 이름이 같아도 상관이 없지만 함수 내부에서는 지역 변수가 우선이라는 규칙이 적용된다.
       * 전역 변수의 값을 변경할 때에는 global 명령어를 사용한다.
         	* 함수 선두에 global 변수 이름으로 쓰면 내부에서 이 변수를 사용할 때 지역 변수를 새로 만들지 않고 전역에 있는 변수를 참조한다.
         	* 원칙적으로는 전역 변수를 자제하는 것이 좋고 이름에 전역임을 표시하여 충돌을 원천적으로 방지하는 것이 합리적이다.



## 문자열

```python
# 문자열을 역순으로 나열할 때
str1 = 'python';

for i in range(len(str1)-1,-1,-1):
    print(str1[i],end=',');
    
# 문자열 추출
print(str1[0:4:2]);
print(str1[2:-2]);
print(str1[0:3]);
print(str1[:3]);
print(str1[3:]);

# 실행결과
n,o,h,t,y,p,
pt
th
pyt
pyt
hon

```

	* 문자열을 앞에서 셀 때에는 0부터 1씩 증가하며 뒤에서 할 때에는 -1부터 1씩 감소한다.
	* 첨자는 반드시 문자열의 길이 범위 내에 있어야 한다.
 * 파이썬에서 문자열은 변경할 수 없는 자료형이다.
   	* 문자열을 바꾸려면 변경된 문자열을 새로 만들어야 한다.
	* 시작 위치를 생략하면 0이 적용되어 문자열의 처음부터 추출한다.

```python
s = 'python programming';
print(s.find('o'));
print(s.rfind('o'));
print(s.index('r'));
print(s.rindex('r'));
print(s.count('o'));

print('a' in s);
print('a' not in s);

if str.startswith(s,'p'):
    print('ok');

if str.endswith(s,'g'):
    print('ok');
    
# 실행결과
4
9
8
11
2
True
False
ok
ok
```

	* find 메서드는 앞에서, rfind는 뒤에서 인수로 지정한 문자 또는 부분 문자열의 위치를 찾는다. 대상 문자가 없으면 두 함수는 모두 -1을 반환한다.
	* index 메서드와 rindex 메서드도 find 메서드와 동일하지만 찾는 문자가 없을 때 예외가 발생한다.
 * count 메서드는 특정 문자나 부분 문자열의 개수를 센다. 해당 문자나 문자열을 발견하지 못하면 0을 반환한다.
   	* find는 첫 번째 발견 위치만 찾지만 count는 전체 문자열을 모두 조사한다.
	* startswith 메서드는 특정 문자열로 시작되는지, endswith 메서드는 특정 문자열로 끝나는지를 조사한다.

```python
str1 = 'python';
str2 = str.upper(str1);
print(str2);

str = '   python   ';
str = str.upper();
print(str.lstrip());
print(str.rstrip());
print(str.strip());

s2 = 'a - b - c - d';
r2 = s2.split('-');
for i in r2:
    print(i.strip());
    
s3 = '[python]';
s4 = s3.replace('[','').rstrip();
s4 = s4.replace(']','').rstrip();
print(s4);

# 실행결과
PYTHON
PYTHON   
   PYTHON
PYTHON
a
b
c
d
python
```

 * swapcase는 대소문자를 반대로 뒤집고 capitalize는 문장의 첫 글자만 대문자로 바꾸며 title은 모든 단어의 처음을 대문자로 변환한다.
   	* 영문자가 아닌 문자는 원래대로 유지된다.
 * strip은 불필요한 공백을 제거할 때 사용한다.
   	* 데이터베이스에 기록하거나 네트워크로 전송할 문자열은 앞뒤 공백을  정리한 후에 사용해야 한다. 그렇지 않으면 기억장소를 낭비할 뿐만 아니라 여러 문제를 유발하기 때문이다.
	* split 메서드는 구분자를 기준으로 문자열을 분할하며 하나의 문자열이 여러 개의 부분 문자열로 쪼개져 리스트에 저장된다. 인수 없이 그냥 호출하면 공백을 기준으로 문자열을 분할한다.
 * replace 메서드는 특정 문자열을 찾아 다른 문자열로 바꾼다.
   	* 첫 번째 인수로 검색할 문자열을 지정하고 두 번째 인수로 바꿀 문자열을 지정한다.

## 포맷팅

