# 클래스

> 1. 클래스 정의
> 2. 활용



##  1. 클래스 정의

```python
class Account:
    def __init__(self,accNo,balance,rate):
        self.accNo = accNo;
        self.balance = balance;
        self.rate = rate;
    def deposit(self, money):
        self.balance += money;
    def withdraw(self, money):
        self.balance -= money;
    def inquire(self):
        return self.balance;

acc1 = Account('111', 20000, 4.5) # acc1이 객체가 된다.
print('잔액은 %d 입니다' % (acc1.inquire())); # 객체에 대한 설계가 필요하다.

acc1.deposit(10000);
print('잔액은 %d 입니다' % (acc1.inquire()));

# 실행결과
잔액은 20000 입니다
잔액은 30000 입니다
```

 * Account라는 이름의 클래스를 정의하고 잔액 속성, 입출금, 조회 동작을 구현한다.
    * 계좌 객체는 속성인 잔액을 각자 따로 가지며 입출력, 조회 메서드는 공유한다.
      	* 사물을 분석하여 클래스로 정의하면 여기서 얼마든지 객체를 만들 수 있다. 잘 만든 객체는 프로그램을 구성하는 부품이 되고 부품이 완벽하면 이를 조립하여 객체 간 관계를 정의하는 식으로 프로그램을 쉽게 완성할 수 있다.
      	* 구조적 방법에 비해 객체 지향 프로그래밍 방법의 생산성이 월등히 높다.

```python
class Account:
    def __init__(self,accNo,balance,rate):
        self.accNo = accNo;
        self.balance = balance;
        self.rate = rate;
    def __str__(self):
        return self.accNo+' '+str(self.balance)+' '+str(self.rate);
    def deposit(self, money): # 객체 지향 함수
        self.balance += money;
    def withdraw(self, money): # 객체 지향 함수
        self.balance -= money;
    def inquire(self): # 객체 지향 함수
        return self.balance;

def getAvg(accList):
    bsum = 0;
    rsum = 0.0;
print('------------------------------------------------------------------------')
import bankapi;
from bankapi import Account;

acc1 = Account('1111',10000,4.5);
print('잔액정보 %s' % (acc1.inquire()));
acc1.withdraw(5000);
print('잔액정보 %s' % (acc1.inquire()));

accList = []; # 계좌를 입력할 리스트
accList.append((Account('1111',10000,3.5)));
accList.append((Account('2222',20000,3.6)));
accList.append((Account('3333',30000,3.7)));
accList.append((Account('4444',40000,3.8)));
accList.append((Account('5555',50000,3.9)));

for acc in accList:
    #print(acc.inquire());
    #print(acc.print());
    print(acc);

# 잔액 평균과 금리 평균을 구하시오
print('잔액 평균: %d, 이자평균: %.2f' % (bankapi.getAvg(accList)));
```

	* 클래스 이름은 대상을 잘 표현하는 이름을 붙이고 첫 글자를 대문자로 적는 것이 관행이다.
 * 클래스를 선언할 때 반드시 정의해야 하는 것은 init과 str이다.
    * init은 생성자( Constructor)이며 객체를 초기화한다.
      	* 파이썬에서는 init에서 초기 값을 받아 대입하는 형식으로 변수와 함수를 생성한다.
      	* 객체를 선언할 때마다 새로운 기억장소가 할당되고 생성자에서 각자의 초기 값을 대입한다.
    * str은 출력하는 모든 정보를 문자열로 나타낸다.
      	* init과 str에서 '_'는 자동으로 함수를 실행하게 하는 기능을 한다.
       * str 대신 print를 사용할 수도 있다.
          * def print(self):
                    return self.accNo + ' ' + str(self.balance) + ' ' + str(self.rate);
	* 

```python
class Account:
    def __init__(self,accNo,balance,rate):
        self.__accNo = accNo;
        self.__balance = balance;
        self.__rate = rate;
    def __str__(self):
        return self.__accNo+' '+str(self.__balance)+' '+str(self.__rate)+' '+str(self.getRateMoney());
    def print(self):
        return self.accNo + ' ' + str(self.balance) + ' ' + str(self.rate);
    def getRate(self):
        return self.__rate; # 객체의 정보를 가져오는 기능
    def setRate(self,rate):
        self.__rate = rate; # 객체의 정보를 설정하는 기능
    def getRateMoney(self):
        m = 0;
        m = (self.__balance * self.__rate) / 100;
        return m;
    def deposit(self, money): # 객체 지향 함수
        self.__balance += money;
    def withdraw(self, money): # 객체 지향 함수
        self.__balance -= money;
    def inquire(self): # 객체 지향 함수
        return self.__balance;
print('---------------------------------------------------')
import bankapi;
from bankapi import Account;

acc1 = Account('1111',10000,3.5);
print(acc1);
print(acc1.inquire());
print(acc1.balance);
print(acc1.getRate());
```

  * 캡슐화(encapsulation)는 객체에 대한 정보의 접근을 함부로 허용하지 않게 하는 기능이다.
    		* 여러 방법으로 할 수 있으나 일반적으로 'self.__변수 이름'으로 나타낸다.
        		* 객체의 특정 정보를 출력하거나 수정하려면 'get변수 이름', 'set변수 이름'으로 클래스 내에 별도로 함수를 만들어야 한다.
		* 캡슐화를 하면 함수를 통해서만 객체의 정보에 접근할 수 있다.

```python
class Human:
    def __init__(self,id,name,salary):
        self.id = id;
        self.name = name;
        self.__salary = salary; # 소득 정보만 incapsulation할 때
    def __str__(self):
        return self.id+' '+self.name+' '+str(self.__salary);
    def getSalary(self):
        return self.__salary;
    def getName(self):
        return self.__name;
    def getId(self):
        return self.__id;
    def setSalary(self, salary):
        if salary < 0:
            return;
        self.__salary = salary;
    salary = property(getSalary,setSalary); 
print(-------------------------------------------------------------------------)
from humanapi import Human;

human = Human('id01','james',3000);
print(human);
# human.salary = 1000000
# human.inner_salary = 2000000

human.setSalary(-4000)

print(human.getSalary());
```

	* 객체의 특정 변수만 캡슐화를 하려면 해당 변수에만 처리를 적용한다.
 * property 함수를 사용하면 캡슐화를 하지 않았을 때 객체의 정보를 접근할 수 있는 경로를 특정 함수로 한정할 수 있다.
   	* property 앞에 있는 변수는 클래스에서 정의한 것과 다르다.
    * salary 대신에 sal을 하면 human.sal로 salary를 설정하거나 출력할 수 있다.
      	* 그러나 이 경우에 다른 사람과 협업을 하는 팀 프로젝트에서는 혼동을 유발할 수 있다.

## 2. 활용

```python
class MinusError(Exception):
    """ Inappropriate argument value (of correct type). """
    def __init__(self, *args, **kwargs): # real signature unknown
        pass

class NotEnoughError(Exception):
    """ Inappropriate argument value (of correct type). """
    def __init__(self, *args, **kwargs): # real signature unknown
        pass

class Account:
    def __init__(self,accNo,balance,rate):
        self.__accNo = accNo;
        self.__balance = balance;
        self.__rate = rate;
    def __str__(self):
        return self.__accNo+' '+str(self.__balance)+' '+str(self.__rate)+' '+str(self.getRateMoney());
    def print(self):
        return self.accNo + ' ' + str(self.balance) + ' ' + str(self.rate);
    def getRate(self):
        return self.__rate;
    def setRate(self, rate):
        self.__rate = rate;
    def getRateMoney(self):
        m = 0;
        m = (self.__balance * self.__rate) / 100;
        return m;
    def deposit(self, money):
        self.__balance += money;
    def withdraw(self, money):
        if money <= 0:
            raise MinusError;
        if self.__balance < money:
            raise NotEnoughError;
        self.__balance -= money;
    def inquire(self):
        return self.__balance;
print('-------------------------------------------------------');
try:
    acc.withdraw(-20000);
except bankapi.MinusError:
    print('입력 금액이 음수입니다');
except bankapi.NotEnoughError:
    print('잔고가 부족합니다')
print(acc);
```

 * 실제 작업에서 클래스를 정의하면서 예상치 못한 오류가 발생할 수 있다. 이를 방지하려면 클래스 정의에서 try ~ except구문을 활용해야 한다.
    * 파이썬에서 정의하지 않은 오류를 직접 정의하려면 기존 오류에 대한 클래스 정의를 이용해야 한다.
      	* crtl + 왼쪽 클릭을 통해 기존 오류에 대한 클래스 정의 파일을 열 수 있다.
      	* 기존 클래스 정의를 복사한 후에 이름을 바꾼 뒤에 init 이후의 내용을 삭제한다.
      	* 작업에 필요한 클래스 정의에서 if와 raise를 이용하여 예상치 못한 오류에 대한 경우를 대처한다.

```python
class Human:
    def __init__(self,name,age):
        self.__name = name;
        if age <= 0:
            self.__age = 1;
        else:
            self.__age = age;
    def __str__(self):
        return self.__name+' '+str(self.__age);
    def print(self):
        return self.__name, self.__age;
    def getAge(self):
        return self.__age;
    def getName(self):
        return self.__name;

class Student(Human):
    def __init__(self,name,age,major):
        super().__init__(name,age);
        self.__major = major;
    def __str__(self):
        return super().__str__()+' '+self.__major;
    def study(self): # student 클래스만 갖고 있는 함수
        return self.__major+'를 공부한다';
    def print(self):
        return super().getName(), super().getAge(), self.__major;
    #def print(self):
        #return super().print() + (self.__major,)
print('------------------------------------------------------------');
class Student(Human):
    def __init__(self,name,age,major):
        super().__init__(name,age);
        self.__major = major;
    def __str__(self):
        return super().__str__()+' '+self.__major;
    def study(self): # student 클래스만 갖고 있는 함수
        return self.__major+'를 공부한다';
    def print(self):
        return super().print() + (self.__major,)
print('------------------------------------------------------------');
import studentapi;
from studentapi import Human, Student;

human = Human('james',20);
print(human);
print('이름 : %s, 나이 : %d' % (human.print()));

st = Student('kim',25,'En')
print(st);
# print('이름 : %s, 나이 : %d' % (st.print())); # 상속을 받았기 때문에 쓸 수 있다.
print(st.print());
print('이름 : %s, 나이 : %d, 전공 : %s' % (st.print()));
print(st.study());
```

 *  상속은 기존 클래스를 확장하여 변수 및 함수를 추가하거나 동작을 변경하는 방법이다.
    	*  상속을 받을 때에는 자식 클래스의 괄호 안에 부모 클래스의 이름을 지정한다.
	*  새로 정의된 자식 클래스는 부모 클래스의 모든 변수와 함수를 물려받으며 이후에 추가로 이를 더 확장하거나 동작을 수정할 수 있다.
	*  한 글래스에서 상속되는 자식 클래스의 개수에는 제한이 없다.
	   *  상속을 받은 자식 클래스가 다른 클래스의 부모가 될 수 있고 파생될수록 해당 클래스는 더 특수한 대상을 표현하기 때문에 속성과 동작이 더 늘어난다.
	*  파이썬은 여러 부모에서 클래스를 파생하는 다중 상속을 지원하지만 이는 프로그램을 복잡하게 만드는 부작용이 있다. 따라서 될 수 있으면 이를 사용하지 않는 것이 바람직하다.

```python
class Car:
    serial = 1000; # Car 클래스에서 공통으로 사용하는 숫자
    def __init__(self,id,name,fsize,cfsize):
        self.__id = id;
        self.__name = name;
        self.__fsize = fsize;
        self.__cfsize = cfsize;
        self.__serial = Car.serial; # 객체에 숫자를 부여할 수 있다.
        Car.serialCount();
    @classmethod # 객체에 숫자를 부여할 수 있다.
    def serialCount(cls):
        cls.serial += 1;
    def print(self):
        return self.__id, self.__name, self.__fsize, self.__cfsize, self.__serial;
    def setCfsize(self, size):
        self.__cfsize += size;
print('--------------------------------------------------------------------------');
class Car:
    serial = 1000; # 클래스의 변수
    def __init__(self,id,name,fsize,cfsize):
        self.__id = id;
        self.__name = name;
        self.__fsize = fsize;
        self.__cfsize = cfsize;
        self.__serial = Car.serial; # 객체의 변수
        Car.serial += 1;
    def print(self):
        return self.__id, self.__name, self.__fsize, self.__cfsize, self.__serial;
    def setCfsize(self, size):
        self.__cfsize += size;
print('--------------------------------------------------------------------------');
import carapi;
from carapi import Car;

car1 = Car('c001','k1',70,10);
print(car1); # 클래스 안에 str을 설정하지 않으면 객체 자체를 문자열 정보로 출력한다.
print('%s %s %d %d %d' % (car1.print()));

car2 = Car('c002','k2',70,10);
print('%s %s %d %d %d' % (car2.print()));
```

 * 클래스에서 init을 정의하기 전에 지역 변수를 삽입하면 이 변수는 클래스에서 공통으로 사용하는 변수가 된다.
    * @classmethod를 쓰고 클래스 변수에 적용할 함수를 정의한다.
      	* 클래스 변수는 cls.변수 이름 또는 클래스 이름.변수 이름으로 나타난다.
      	* 함수를 정의한 경우에는 init 정의에 클래스 이름.함수 이름()을 추가한다.
	* str대신에 print함수를 정의할 경우에 출력되는 것은 튜플(tuple)이므로 수정이나 삭제를 위해선 리스트와 같은 다른 구조로 바꾸어야 한다.

```python
class sql:
    insertUser = 'INSERT INTO USERDB';
    selectUser = 'SELECT * FROM USERDB';
    insertItem = 'INSERT INTO ITEMDB';
    selectItem = 'SELECT * FROM ITEMDB';

class UserDb:
    @staticmethod
    def insert():
        print(sql.insertUser);

    @staticmethod
    def select():
        print(sql.selectUser);

class ItemDb:
    @staticmethod
    def insert():
        print(sql.insertItem);

    @staticmethod
    def select():
        print(sql.selectItem);
print('-------------------------------------------');
from dbutil import ItemDb, UserDb;
UserDb.insert();
UserDb.select();

ItemDb.insert();
ItemDb.select();

# 실행결과
INSERT INTO USERDB
SELECT * FROM USERDB
INSERT INTO ITEMDB
SELECT * FROM ITEMDB
```

 * 정적 메서드는 글래스에 포함되는 단순 유틸리티 메서드이다. 특정 개체에 소속되지 않고 클래스와 관련한 동작을 하지도 않는다.
   	* 객체를 생성할 필요가 없으며 함수만 쓰고 싶을 때 정의하기 전에 @staticmethod를 입력한다.

```python
import math
from array import array
from decimal import Decimal;
from fractions import Fraction;

n = 0.1;
n = Decimal(n);
sum = 0;
for i in range(1,11):
    sum += n;
print(sum);

n = 0.1;
n = Decimal(str(n));
sum = 0;
for i in range(1,11):
    sum += n;
print(sum);

result = 1/3;
print(result);

result = round(result,3);

# 실행결과
1.000000000000000055511151231
1.0
0.3333333333333333
0.333
7/12
0.5833333333333334
100,3,4,3,2,1,200,100,4,2,200,
Process finished with exit code 0
```

