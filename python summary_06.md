# 객체를 활용한 데이터베이스

> 1. SQLite
> 2. Value
> 3. App



```python
import sqlite3;

from value import user


class Sql:
    makeusertb = """CREATE TABLE IF NOT EXISTS usertb(
                    id CHAR(10) PRIMARY KEY,
                    pwd  CHAR(10),
                    name CHAR(10),
                    age  NUMBER(3)
    )""";
    insertUser = """INSERT  INTO  usertb VALUES ('%s','%s','%s',%d)""";
    deleteUser = """DELETE  FROM   usertb  WHERE  id = '%s'""";
    updateUser = """UPDATE  usertb  SET  pwd='%s', name='%s', age=%d  WHERE  																				id='%s'""";
    selectUser = """SELECT  *  FROM  usertb   WHERE  id='%s'""";
    selectAllUser = """SELECT  *  FROM  usertb""";
```

 * App 파일이 데이터베이스를 이용할 수 있도록 도와주는 기능을 한다.
 * SQLite 구문을 클래스의 정적 메서드로 저장하여 복잡한 코드 입력을 단순하게 한다.

```python
class SqliteDb:
    def __init__(self, dbName):
        self.__dbName = dbName;
    def getConnect(self):
        con = sqlite3.connect(self.__dbName);
        cursor = con.cursor();
        #print(self.__dbName+ ' Connected ...');
        return {'con':con, 'cursor':cursor};

    def close(self, cc):
        if cc['cursor'] != None:
            cc['cursor'].close();
        if cc['con'] != None:
            cc['con'].close();

    def makeTable(self):
        "Make usertb Table"
        cc = self.getConnect();
        cc['cursor'].execute(Sql.makeusertb);
        cc['con'].commit();
        self.close(cc);

    def insert(self,u):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.insertUser % u.sqlmap());
        cc['con'].commit();
        self.close(cc);

    def delete(self,id):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.deleteUser % (id));
        cc['con'].commit();
        self.close(cc);

    def update(self, u):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.updateUser % (u.pwd, u.name, u.age, u.id));
        cc['con'].commit();
        self.close(cc);

    def select(self):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.selectAllUser);
        result = cc['cursor'].fetchall();
        userall = [];
        for u in result:
            # ('','','',20)
            tu = user(u[0],u[1],u[2],u[3]);
            userall.append(tu);
        self.close(cc);
        return userall;

    def selectone(self,id):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.selectUser % (id));
        #('','','',39)
        uu = cc['cursor'].fetchone();
        us = user(uu[0],uu[1],uu[2],uu[3]);
        self.close(cc);
        return us;
```

 * App과 데이터베이스를 연결할 동작을 작성한다.
   	* init을 이용하여 데이터베이스 이름을 지정하는 함수를 만든다.
   	* 데이터베이스를 접속하는 함수를 만들 때, 'con'과 'cursor'를 전역 변수로 설정하지 않고 사전으로 출력한다.

```python
# 주문번호 구하는 함수
    def serialCnt(self):
        cc = self.connectDB()
        cc['cursor'].execute(Sql.selectAll);
        allorders = cc['cursor'].fetchall();

        list = []
        if len(allorders) == 0:
            serial = self.dateNow() + str(1)
        else:
            for i in range(len(allorders)):
                list.append(int(allorders[i][0][8:]));
            serial = self.dateNow() + str(max(list) + 1);
        self.closeDB(cc)
        return serial;
```



```python
class user: #user object
    def __init__(self, id, pwd, name, age):
        self.__id = id;
        self.__pwd = pwd;
        self.__name = name;
        self.__age = age;
    def __str__(self):
        return self.__id+' '+self.__pwd+' '+self.__name+' '+str(self.__age)+' ';
    def sqlmap(self):
        return self.__id, self.__pwd, self.__name, self.__age;
    def getId(self):
        return self.__id;
    def setId(self, id):
        self.__id = id;
    def getName(self):
        return self.__name;
    def setName(self, name):
        self.__name = name;
    def getPwd(self):
        return self.__pwd;
    def setPwd(self, pwd):
        self.__pwd = pwd;
    def getAge(self):
        return self.__age;
    def setAge(self, age):
        self.__age = age;
    id = property(getId, setId);
    pwd = property(getPwd, setPwd);
    name = property(getName, setName);
    age = property(getAge, setAge);
```

 * property 설정을 하면 외부에서 클래스 내부 변수를 참조할 수 있다.
   	* 캡슐화는 클래스에 특성과 의미를 부여해서 하나의 객체를 만드는 것을 의미한다.
    * 외부에서 변수를 마음대로 조작할 수 있으면 클래스가 정해놓은 규칙대로 작동하지 않고 에러가 날 가능성이 크다.
      	* 따라서 갭슐화를 통해 내부에서 사용하는 변수와 외부에서 사용하는 함수를 철저하게 구분한다.

```python
from sqlitedb import *;
from value import *;

print('start ....');

sqldb = SqliteDb('udb.db');
sqldb.makeTable();
u = user('id05','pwd05','james',28);
#sqldb.insert(u);
userlist = sqldb.select();
for us in userlist:
    print(us.id+' '+us.name+' '+str(us.age));
print('---------------------------------');
userone = sqldb.selectone('id01');
print(userone);

sqldb.delete('id02');
userlist = sqldb.select();
for us in userlist:
    print(us.id+' '+us.name+' '+str(us.age));
print('---------------------------------');

print('end ....');

from sqlitedb import *;
from value import *;

sqlitedb = SqliteDb('udb.db');

userlist = sqlitedb.select();
for u in userlist:
    print(u);

us = user('id01','1111','kim',30);
sqlitedb.update(us);
print('-------------------------------------');

userlist = sqlitedb.select();
for u in userlist:
    print(u);
```



```python
from orderutil_class import OrderDb;
from ordervalue import Order;

print('Start ....')

orderDb = OrderDb('order.db')
orderDb.makeTable()

while True:
    menu = input('***주문테이블구성***\n'
                 '주문추가(i)\n'
                 '주문정보전체조회(a)\n'
                 '주문번호조회(s)\n'
                 '주문삭제(d)\n'
                 '주문수정(u)\n'
                 '나가기(q)');
    menu = menu.lower()
    if menu == 'q':
        print('Bye')
        break

    #주문 추가
    if menu == 'i':
        orderInfo = input('추가할 주문정보를 입력하세요\n'
                     'userid, itemid, price, qt\n'
                     '(띄어쓰기기준)').split(' ')
        serialNum = orderDb.serialCnt()
        orderObj = Order(serialNum,
                         serialNum[:8],
                         orderInfo[0],
                         orderInfo[1],
                         int(orderInfo[2]),
                         int(orderInfo[3]))
        orderDb.insertOrder(orderObj)

    #주문정보 전체 조회
    if menu =='a':
        allOrders = orderDb.selectOrder()
        for o in allOrders:
            print("주문번호: %s,주문날짜: %s,사용자id: %s,제품id: %s,가격: %s,수량: %s" % (o.id,
                                         o.date,
                                         o.userid,
                                         o.itemid,
                                         o.price,
                                         o.qt))

    #주문정보 한개 조회
    if menu == 's':
        id = input('조회하실 주문번호를 입력해주세요')
        oneOrder = orderDb.selectOneOrder(id)
        print("주문번호: %s,주문날짜: %s,사용자id: %s,제품id: %s,가격: %s,수량: %s" % (oneOrder.id,
                                                                      oneOrder.date,
                                                                      oneOrder.userid,
                                                                      oneOrder.itemid,
                                                                      oneOrder.price,
                                                                      oneOrder.qt))

    # 주문정보 수정
    if menu == 'u':
        orderInfo = input('수정하실 주문번호와 정보를 입력해주세요\n'
                     'id, userid, itemid, price, qt\n'
                     '(띄어쓰기기준)').split(' ')
        orderObj = Order(orderInfo[0],
                         orderInfo[0][:8],
                         orderInfo[1],
                         orderInfo[2],
                         int(orderInfo[3]),
                         int(orderInfo[4]))
        orderDb.updateOrder(orderObj)

    # 주문 삭제
    if menu == 'd':
        id = input('삭제하실 주문번호를 입력하세요')
        orderDb.deleteOrder(id)


print('End ....')
```

