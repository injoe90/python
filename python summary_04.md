# 파일 입출력

> 1. 파일 입출력
> 2. 데이터베이스



## 1. 파일 입출력

```python
f = open('live.txt','wt', encoding = 'UTF-8');
f.write("""한글 테스트 한글테스트
한글 테스트  한글 테스트  한글테스트
한글테스트   한글테스트   한글테스트
""");
f.close() # 파일을 입출력하고 나서는 반드시 close 메서드로 닫아 정리해야 한다.

f = None;
try:
    f = open('live.txt','rt',encoding='UTF-8'); # 모드 뒤에는 파일 종류를 지정한다.
    text = f.read(); # 문서 파일의 크기가 클 때에는 읽는 속도가 느려진다.
    print(text);
except:
    print('File Not Found...');
finally:
    if f != None:
        print('Close...');
        f.close();
```

	* 파일 경로는 입출력 대상 파일의 이름이고 파일명만 있으면 현재 디렉토리에서 이를 찾는다.
	* 모드에서 읽기, 쓰기, 추가 등을 지정한다.

| 모드 |                       설명                        |
| :--: | :-----------------------------------------------: |
|  r   | 파일을 읽으며 만약 이것이 없으면 예외가 발생한다. |
|  w   |   파일에 기록하며 파일이 이미 있으면 덮어쓴다.    |
|  a   |             파일에 데이터를 추가한다.             |
|  x   |       파일에 기록하되 이미 있으면 실패한다.       |

	* 파일의 종류를 지정할 때 t는 텍스트 파일, b는 이진 파일을 의미한다.
	* 파일을 입출력할 때에는 반드시 마지막에 close 메서드로 닫아 정리해야 하므로 try ~ except ~ finally 구문을 활용한다.
	* 한글로 작성한 파일을 온전하게 출력하려면 open 함수에서 encoding='UTF-8'을 덧붙인다.

```python
print('start...');
f = None;
try:
    f = open('live.txt','rt',encoding='UTF-8');
    for text in f:
        print(text, end = '');
except:
    print('Error...');
finally:
    if f != None:
        f.close();
print('end...');
print('------------------------------------------')
print('start...');
f = None;
try:
    f = open('live.txt','rt',encoding='UTF-8');
    while True:
        text = f.readline(); # 1줄씩 읽는다.
        if not text:
            break;
except:
    print('Error...');
finally:
    if f != None:
        f.close();
print('end...');
```

	* 출력하려는 파일의 크기가 클 때, 한꺼번에 읽으면 처리속도가 상당히 느릴 수 있다. 이에, for문이나 while True문을 이용하여 한 줄씩 읽을 수 있다.

```python
f = None;
f = open('text.txt','rt',encoding='UTF-8');
f.seek(6,0);
text = f.read();
print(len(text)); # 한글을 읽을 때에 개수로 파악한다.
print(text);
f.close();
```

 * f.seek(n, 0) 함수는 n바이트 이후에 있는 글을 읽는다.
   	* UTF-8에서는 한글을 3바이트로 인식하므로 n=4로 지정하면 글자가 깨지고 이를 처리할 수 없어 에러가 발생한다.



## 2. 데이터베이스

> 1) 테이블 생성
>
> 2) 조회
>
> 3) 수정 및 삭제



```python
import sqlite3
print('strat...');
con = None;
cursor = None;

# 1. SQLite에 접속한다.
con = sqlite3.connect('addr.db');
cursor = con.cursor();
print('SQLite Connected..');

# 2. Table을 만든다.
cursor.execute('DROP TABLE IF EXISTS users'); # 이미 만들어진 테이블이 있으면 수정할 수 없으므로 새로 만든다.
cursor.execute("""CREATE TABLE IF NOT EXISTS users
   (id CHAR(16) PRIMARY KEY,
    pwd CHAR(16),
    name CHAR(10),
    phone CHAR(15),
    addr CHAR(20),
    age NUMBER(3)) """);
con.commit();
print('Table Created..');

# 3. 사용자 정보를 입력한다.
cursor.execute("""INSERT INTO users VALUES
('id01','pwd01','이말숙','01099992222','서울',29)
"""); # SQL문에서 문자는 항상 작은 따옴표를 사용한다.
con.commit(); # 실행을 할 때마다 입력한다. / 정보의 변경이 있을 때에는 항상 입력해야 한다.

# 4. 사용자 정보를 조회한다.
cursor.execute('SELECT * FROM users'); # users 테이블에서 모든 정보를 가져온다. / commit()을 하지 않는다.
allusers = cursor.fetchall(); # allusers 변수에 모든 정보가 입력된다.
for user in allusers:
    print('%s %s %s %s %s %d' % (user[0],user[1],user[2],user[3],user[4],user[5]));

#5 SQLite와 접속을 끊는다.
cursor.close();
con.close();

print('end...');
```

 * SQLite를 이용하여 데이터베이스를 다룰 때에는 다음과 같은 단계를 염두에 두어야 한다.
   	* (1) SQLite에 접속한다.
   	* (2) Table을 만든다.
   	* (3) 사용자 정보를 입력한다.
   	* (4) 사용자 정보를 조회한다.
   	* (5) SQLite와 접속을 끊는다.
	* 파일 입출력 작업과 마찬가지로 반드시 (5) 작업이 이루어져야 한다. 이를 위해 try ~ except ~ finally 구문을 활용할 수 있다.

```python
import sqlite3
print('strat...');
con = None;
cursor = None;

# 1. SQLite에 접속한다.
con = sqlite3.connect('addr.db');
cursor = con.cursor();
print('SQLite Connected..');
print('-------------------------------');

import sqlite3;
con = None; # 전역 변수
cursor = None; # 전역 변수

def connectDB(dbName):
    "Connect SQLite..."
    global con, cursor; # 변수를 초기화하기 위해 필요하다.
    con = sqlite3.connect(dbName);
    cursor = con.cursor();
print('----------------------------------------------');    

import sqlite3;
import dbutil;

print('Start...')
dbutil.connectDB('addr.db');
```

	* connect 메서드는 데이터베이스를 연다.
 * cursor 메서드는 SQLite문을 실행하고 결과를 읽는 객체이다.
   	* 커서의 execute 메서드로 SQLite 명령을 수행한다.
	* 사용이 끝나면 커서와 연결 객체를 닫아 자원을 정리한다.

### 1) 테이블 생성

```python
cursor.execute('DROP TABLE IF EXISTS users');
cursor.execute("""CREATE TABLE IF NOT EXISTS users
   (id CHAR(16) PRIMARY KEY,
    pwd CHAR(16),
    name CHAR(10),
    phone CHAR(15),
    addr CHAR(20),
    age NUMBER(3)) """);
con.commit();
```

	* SQLite에 IF NOT EXISTS를 넣으면 이미 생성한 테이블을 반복해서 만들지 않는다.
	* SQLite는 대문자와 소문자를 구분하지 않는다.
	* 이미 생성된 테이블은 수정할 수 없으므로 이를 지우고 조건에 맞게 다시 만들어야 한다.
	* 테이블의 정보가 바뀔 때에는 항상 con.commit()을 입력해야 한다.
	* 자료를 기록하는 개체 간 구분의 기준이 되는 변수에는 PRIMARY KEY를 변수 이름 옆에 덧붙인다.

```python
# 3. 사용자 정보를 입력한다.
cursor.execute("""INSERT INTO users VALUES
('id01','pwd01','이말숙','01099992222','서울',29)
""");
con.commit();
print('------------------------------------------------------')
def insertUser(user):
    "Insert User Data"
    insertSQL = """INSERT INTO users VALUES ('%s','%s','%s','%s','%s','%d')""" % \
                (user[0], user[1], user[2], user[3], user[4], user[5]);
    cursor.execute(insertSQL);
    con.commit();
def insertUser(user):
    "Insert User Data"
    insertSQL = \
        "INSERT INTO users VALUES ('"+\
        user[0]+"','"+user[1]+"','"+\
        user[2]+"','"+user[3]+"','"+\
        user[4]+"',"+str(user[5])+")";
    cursor.execute(insertSQL);
    con.commit();

user = ['id09','pwd09','홍말숙','01077776666','경기',20];
dbutil.insertUser(user);
```

	* SQLite에서 문자에는 항상 작은 따옴표를 사용한다.
	* 함수를 정의할 때에는 숫자형 변수도 선형 포맷팅에서 작은 따옴표를 추가한다.

### 2) 조회

```python
cursor.execute('SELECT * FROM users');
allusers = cursor.fetchall();
for user in allusers:
    print('%s %s %s %s %s %d' % (user[0],user[1],user[2],user[3],user[4],user[5]));

userId = input('Input Id..');
oneUser = dbutil.selectOneUser(userId.strip());
print(oneUser);
print('----------------------------------------------------------------------------')    
def selectUser():
    "Select User Data"
    allusers = [];
    selectSQL = """SELECT * FROM users""";
    users = cursor.execute(selectSQL);
    for u in users:
        user = [];
        user.append(u[0]); # ID
        user.append(u[1]); # PWD
        user.append(u[2]); # NAME
        user.append(u[3]); # PHN
        user.append(u[4]); # ADDR
        user.append(int(u[5])); # AGE
        allusers.append(user);
    return allusers;

def selectOneUser(id):
    # "Select One User"
    user = [];
    selectOneSQL = """SELECT * FROM users WHERE id='%s'""" % (id);
    cursor.execute(selectOneSQL);
    userData = cursor.fetchone();
    user.append(userData[0]);
    user.append(userData[1]);
    user.append(userData[2]);
    user.append(userData[3]);
    user.append(userData[4]);
    user.append(str(userData[5]));
    return user;
```

	* 정보가 바뀌지 않기 때문에 con.commit()을 입력하지 않는다.
 * 모든 DBMS에서는 출력하려는 자료가 시작하는 위치 이전에 커서가 있다.
   	* 모든 정보를 조회할 때 사용하는 fetchall과 달리 특정 개체를 출력할 때 필요한 fectcone은 커서를 자료가 있는 곳으로 이동시킨다. 따라서 다른 명령어를 통해 자료를 출력할 수 있다.
	* input함수로 인수를 입력할 때에는 strip함수를 이용하여 입력할 때 발생할 수 있는 공백을 제거한다.

### 3) 수정 및 삭제

```python
def deleteUser(id):
    # "Delete One User"
    deleteSQL = """DELETE FROM users WHERE id='%s'""" % (id);
    cursor.execute(deleteSQL);
    con.commit();

def updateUser(user):
    # "Update One user"
    updateSQL = """UPDATE users SET pwd='%s',name='%s',phone='%s',addr='%s',age=%d WHERE id='%s'""" \
                % (user[1], user[2], user[3], user[4], user[5], user[0]);
    cursor.execute(updateSQL);
    con.commit();

#3-2. 수정
user = ['id01','111111','장말숙','01077776666','경기',20];
dbutil.updateUser(user);

#3-3. 삭제
dbutil.deleteUser('id08');
```

	* 삭제와 관련한 SQLite는 삭제하려는 자료가 데이터베이스에 없다면 관련 명령을 무시한다.
	* 수정할 때 개체 식별을 위한 변수는 마지막 순서로 이동시킨다.



