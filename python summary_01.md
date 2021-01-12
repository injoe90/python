# IT 개발언어 학습단계

> 1. 개발환경
> 2. 구조와 문법
> 3. 변수
> 4. 반복문, 조건문 
> 5. 함수
> 6. 객체지향
> 7. 알고리즘



## 변수

```python
price = input('가격을 입력하세요 : ');
num = input('개수를 입력하세요 : ');
sum = int(price) * int(num);
print('총액은 ',sum,'원 입니다');
print('총액은 ' + str(sum) + '원 입니다');

# 실행결과
가격을 입력하세요 : 100
개수를 입력하세요 : 5
총액은  500 원 입니다
총액은 500원 입니다
```

	* input 함수로 받은 값은 항상 문자열이다.
	* 모든 명칭은 대소문자를 구분한다.
	* 여러 단어로 구성된 명칭은 공백으로 띄울 수 없으므로 중간에 '_'을 넣거나 단어의 첫 글자를 대문자로 쓴다.



## 조건문과 반복문

> 1. if
> 2. for
> 3. while
> 4. while True



```python
a = input("Input Number .. ?");
3;
if int(a) > 10 :
    print("Bin Number");
if int(a) > 5 :
    print("Middle Number");
if int(a) > 1 :
    print("Small Number");
print("end");

print('++++++++++++++++')

if int(a) > 10 :
    print("Bin Number");
elif int(a) > 5 :
    print("Middle Number");
elif int(a) > 1 :
    print("Small Number");
else:
    print("invalid Number ,,, !")
print("end");

print('++++++++++++++++')

if int(a) % 2 == 0:
    print("짝수");
else:
    print("홀수");
```

 * 단순 if문은 조건 진위에 따라 참이면 명령을 실행하고 거짓이면 아무것도 하지 않는다.

   	*  if else문은 조건 진위에 따라 실행할 명령을 선택한다.

   * 하나의 조건만 보는 것이 아니라 다른 세부 조건을 더 점검할 때 elif문을 활용한다.
     * elif는 조건을 반복적으로 점검하여 상황에 맞는 명령을 선택한다.

```python
gender = "m";
age = 20;

if gender == "m":
    if age > 19:
        print("남자성인");
    else:
        print("남자소년");
else:
    if age > 19:
        print("여자성인");
    else:
        print("여자소녀");
```

*  if문의 조건에 걸리는 명령의 종류에는 제한이 없다.
   *  if문끼리 중첩하면 두 조건이 모두 참일 때에만 명령을 실행한다. and 논리 연산자로도 이를 비슷하게 나타낼 수 있다.
      *  if if문은 두 조건을 순서대로 점검하는 것이고 if and문은 두 조건을 한꺼번에 점검하지만 두 조건문의 효과는 완전히 같다. 다만 if and일 때 복잡하며 불필요한 코드를 반복하면 구조가 나빠져 수정하기 번거로워진다.

```python
sum = 0;
cnt = 0;
for n in range(1,11) :
    sum += n;
    cnt += 1;
print(sum);
print(cnt);

list = [];
for n in range(1,100,5):
    list.append(n);
print(list);

# 실행결과
5
10
[1, 6, 11, 16, 21, 26, 31, 36, 41, 46, 51, 56, 61, 66, 71, 76, 81, 86, 91, 96]
```

 * for문은 모임(collection)의 요소를 순서대로 반복하면서 루프의 명령을 실행하는 반복문이다.
    * 파이썬에서는 모든 순서를 0에서 시작한다.
      	* 모임인 range(1, m)은 1번 째부터 시작하며 m-1까지 범위를 지정한다.
      	*  range(int(m))의 범위도 1에서 m-1까지이다.
      	*  range(a, b, c)은 a에서 b까지 요소를 나열하되 이를 c만큼 더하여 나타낸다. 
	* 일정 범위의 리스트를 만들고 요소를 반복한다. 이후에, range 명령을 통해 순차적으로 증가하는 리스트를 만든다.

```python
for n in range(2,10):
    if n % 2 == 1:
        continue
    print(str(n)+"단");
    for d in range(1,10):
        print(n,d,n*d,sep="-");
print("-----------------------------------------------")

for n in range(2,10):
    if n >6:
        break;
    if n % 2 == 1:
        continue
    print(str(n)+"단");
    for d in range(1,10):
        print(n,d,n*d,sep="-");
        if n == 6 and d == 5:
            break
```

 * for문을 2중, 3중으로 반복하는 것은 2차원, 3차원 배열을 활용하는 것과 같다.
    * 반복 과정에서 어떤 이유로 이를 중지하거나 건너뛰어야 할 경우가 있을 때 흐름 제어문을 사용한다.
      	* break 명령은 반복문을 끝내고 다음 명령으로 이동한다. 무조건 탈출하는 것이 아니라 특정 조건에 의해 벗어나므로 보통 if문과 함께 사용한다.
      	* continue 명령은 현재 반복만 중지하고 루프의 선두로 돌아가 조건을 점검한 후에 다음 번 반복을 계속 수행한다. 보통 if문과 함께 사용한다.

```python
sum2 = 0;
cnt = 0;
while cnt <= 10 :
    sum2 += cnt;
    cnt += 1;
print(sum2);
print(cnt);

# 실행결과
55
11
```

 * while문으로 만든 코드를 for문으로 바꿀 수 있으며 반대도 가능하다. 다만, 두 반복문은 서로 장단점이 있어 용도에 따라 구분한다.
   	* for문은 반복 범위가 명확한 경우에 주로 사용한다.
    * while문은 반복 범위가 가변적인 경우에 주로 활용한다.
      	* for문은 시작과 끝이 결정되어 있지만 while문은 시작을 선언해야 한다.

```python
print('start......');
while True:
    data = input('input number...?[q:quit]');
    if data.lower() == 'q':
        print('bye..');
        exit();
    if data.isdecimal():
        result = int(data) * 1000;
        print(result);
    else:
        print('invalid value.. try again');

print('end........');
```

 * 무한 루프는 횟수를 정하지 않고 계속 반복하며 중간에서 조건을 점검하여 탈출하는 형식이다.
   	* for문으로는 무한 루프를 만들 수 없다.
   	* 진짜 무한히 반복해서는 안 되므로 break 명령이나 exit()을 통해 적당한 조건에서 끝내야 한다.

### Workshop

```python
data = [
    [100,90,98,88],
    [100,90,98,87],
    [100,90,98,86],
    [100,90,98,85]
]

# 각 학생의 합과 평균을 출력 #
# 전체 학생의 합과 평균을 출력 #

allsum = 0
for n in range(len(data)):
    sum = 0
    cnt = 1
    for d in data[n]:
        sum += d
        cnt += 1
    allsum += sum
    print("학생 ",str(n+1),"의 ","점수 합 :",sum);
    print("학생 ",str(n+1),"의 ","평균 :",sum/cnt);
print("전체 합:",allsum);
print("전체 평균",allsum/len(data))

# 실행결과
학생  1 의  점수 합 : 376
학생  1 의  평균 : 75.2
학생  2 의  점수 합 : 375
학생  2 의  평균 : 75.0
학생  3 의  점수 합 : 374
학생  3 의  평균 : 74.8
학생  4 의  점수 합 : 373
학생  4 의  평균 : 74.6
전체 합: 1498
전체 평균 374.5
```

