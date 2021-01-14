# 객체를 활용한 데이터베이스 (1)

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



# 객체를 활용한 데이터베이스 (2)

> 1. SQLite
> 2. Value



```python
import sqlite3;

class Sql:
    makeusertb = """
    CREATE TABLE IF NOT EXISTS usertb(
                    id CHAR(10) PRIMARY KEY,
                    pwd  CHAR(10),
                    name CHAR(10),
                    age  NUMBER(3)
    )""";
    makeitemtb = """CREATE TABLE IF NOT EXISTS itemtb(
        id CHAR(10) PRIMARY KEY,
        name CHAR(10),
        price NUMBER(10)
    )
    """;
    insertUser = """INSERT  INTO  usertb VALUES ('%s','%s','%s',%d)""";
    deleteUser = """DELETE  FROM   usertb  WHERE  id = '%s'""";
    updateUser = """UPDATE  usertb  SET  pwd='%s', name='%s', age=%d  WHERE  																		id='%s'""";
    selectUser = """SELECT  *  FROM  usertb   WHERE  id='%s'""";
    selectAllUser = """SELECT  *  FROM  usertb""";

    insertItem = """INSERT  INTO  itemtb VALUES ('%s','%s', %d)""";
    deleteItem = """DELETE  FROM  itemtb  WHERE  id = '%s'""";
    updateItem = """UPDATE  itemtb  SET  name='%s', price=%d  WHERE  id='%s'""";
    selectItem = """SELECT  *  FROM  itemtb   WHERE  id='%s'""";
    selectAllItem = """SELECT  *  FROM  itemtb""";

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
        cc['cursor'].execute(Sql.makeitemtb);
        cc['con'].commit();
        self.close(cc);
```

	* 만들고자 하는 테이블의 개수가 2개 이상일 때, SQLite문을 부모 클래스로 하고 각 테이블을 다루는 데이터베이스 객체를 자식 클래스로 한다.
 * SqliteDb에서는 사용할 데이터베이스의 이름을 지정하고 자식 클래스에서 활용할 데이터베이스 접속과 차단에 활용할 함수와 테이블 생성 함수를 만든다.
   	* 테이블 생성 함수에서는 커서의 실행 명령 수를 만들려는 테이블 수에 맞게 늘린다.

```python
from sqlitedb import *
from value import User

class UserDb(SqliteDb):

    def __init__(self, dbName):
        super().__init__(dbName);

    def insert(self, u):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.insertUser % u.sqlmap());
        cc['con'].commit();
        self.close(cc);

    def delete(self, id):
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
        all = [];
        for u in result:
            # ('','','',20)
            tu = User(u[0], u[1], u[2], u[3]);
            all.append(tu);
        self.close(cc);
        return all;

    def selectone(self, id):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.selectUser % (id));
        # ('','','',39)
        obj = cc['cursor'].fetchone();
        result = User(obj[0], obj[1], obj[2], obj[3]);
        self.close(cc);
        return result;
print('-------------------------------------------------');
from sqlitedb import *
from value import Item

class ItemDb(SqliteDb):
    def insert(self, item):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.insertItem % item.sqlmap());
        cc['con'].commit();
        self.close(cc);

    def delete(self, id):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.deleteItem % (id));
        cc['con'].commit();
        self.close(cc);

    def update(self, item):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.updateItem % (item.name, item.price, item.id));
        cc['con'].commit();
        self.close(cc);

    def select(self):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.selectAllItem);
        result = cc['cursor'].fetchall();
        all = [];
        for u in result:
            # ('','','',20)
            tu = Item(u[0], u[1], u[2]);
            all.append(tu);
        self.close(cc);
        return all;

    def selectone(self, id):
        cc = self.getConnect();
        cc['cursor'].execute(Sql.selectItem % (id));
        # ('','','',39)
        obj = cc['cursor'].fetchone();
        result = Item(obj[0], obj[1], obj[2]);
        self.close(cc);
        return result;
```

 *  자식 클래스에서 부모의 메서드를 호출할 때에는 super 메서드를 이용한다.
    	*  자식 클래스의 init은 인수로 받은 변수를 초기화하기 위해 부모의 생성자인 'supper().__init__'을 호출한다.
    	*  부모의 생성자를 호출하는 대신에 직접 변수와 함수를 초기화할 수 있지만 부모 클래스가 변경될 때 자식 클래스도 같이 수정해야 한다. 따라서 상속받은 클래스는 부모의 생성자에게 초기화를 위임하고 직접 추가한 변수와 함수만 초기화하는 것이 정석이다.
	*  자식 클래스에서 __init__ 메서드를 생략하면 부모 클래스의 __init__을 자동으로 호출하므로 super 메서드를 사용하지 않아도 된다.



```python
from itemdb import ItemDb
from userdb import *;

#1. 사용하고자 하는 데이터베이스 이름을 설정한다,
from value import Item

sqlitedb = SqliteDb('shopdb.db');

#2. 테이블을 생성 한다, 단, 존재 하지 않으면
sqlitedb.makeTable();

#3. 사용자 테이블을 사용하기 위해 userdb 객체를 이용하여 'CRUD'를 진행
udb = UserDb('shopdb.db');
user = User('id04','pwd04','james',20);
#udb.insert(user);

userlist = udb.select();
for u in userlist:
    print(u);
```



