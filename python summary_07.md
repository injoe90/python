# 파이썬 여러 기능

> 1. 모듈 경로
> 2. eval과 repr
> 3. pip



## 1. 모듈 경로

```python
import sys;
sys.path.append('C:\\mylib');
import one.util.util;
print(one.util.util.calcsum(555));
cal = one.util.util.Calc(333);
print(cal.calc());
print('---------------------------------');

import sys;
sys.path.append('C:\\mylib');
from one.util.util import *;
print(calcsum(555));
cal = Calc(333);
print(cal.calc());
```

 * 외부에서 개발한 모듈이나 사용자 정의 모듈을 사용하려면 호출하려는 파일과 같은 디렉토리에 있어야 한다.
   	* 위의 방법을 이용하면 다른 디렉토리에 있는 모듈을 이용할 수 있다.
   	* 그러나 될 수 있으면 사용하려는 모듈을 현재 경로에 두는 것이 바람직하고 가장 쉽다. 또한 추가 패키지를 설치하는 site-package 폴더에 자주 사용하는 모듈을 복사하는 것도 고려할 수 있다. 이 폴더는 기본 경로에 포함되어 있어 패스를 추가하지 않아도 항상 동작된다.



## 2. eval과 repr

```python
str = '3+4+5';
strlist = '[1,2,3,4,5]';

print(eval(str));
print(strlist);

for i in eval(strlist):
    print(i);
    
# 실행결과
12
[1,2,3,4,5]
1
2
3
4
5
```

 * eval 함수는 문자열 형태로 된 파이썬 표현식을 평가하여 결과를 반환한다. 실행 중에 만든 문자열을 넘기면 해석기가 코드를 분석하고 실행하는 방식이다. 이 함수를 사용하면 실시간으로 코드를 만들어 실행할 수 있다.
   	* 문자열 내의 표현식에 에러가 있다면 이것도 실시간으로 처리된다.
   	* eval의 인수는 문자열이며 실시간으로 조립할 수도 있고 사용자가 입력할 수도 있다.
 * repr 함수는 eval과 반대로 동작하며 객체에서 문자열 표현식을 생성한다.
   	* str 함수는 사람이 읽기 쉽게 객체의 냉요을 보여주는 것이 목적이므로 원래 모습을 유지한다.
    * repr 함수는 해석기를 위한 표현식을 만드므로 이 표현식으로 객체를 다시 만들 수 있어야 한다.
      	* str 함수와 달리 repr 함수는 문자열 안에 문자열을 담아 표현한다.
	* 이 기능을 잘 사용하면 객체를 표현식으로 바꾸어 네트워크로 전송하거나 데이터베이스에 저장할 수 있고 표현식에서 객체를 다시 만들 수도 있다. 따라서 객체를 자유롭게 관리할 수 있다.



## 3. pip

```python
from urllib import request;
import bs4;

target = request.urlopen("https://www.kma.go.kr/wether/forecast/mid-term-rss3.jsp?stnId=108");
soup = bs4.BeautifulSoup(target,'html.parser');
locations = soup.select('location');

for city in locations:
    name = city.select_one('city').string
    wf = city.select_one('wf').string
    tmn = city.select_one('tmn').string
    tmx = city.select_one('tmx').string
    print('%s : %s(%s ~ %s)' % (name, wf, tmn, tmx));
```

 * 터미널에 pip installer 설치하려는 패키지를 입력한다.
   	* pip freeze : 설치한 패키지의 목록을 보여준다.
   	* pip installer 패키지 이름 : 패키지를 설치한다.
	* BeautifulSoup는 HTML이나 XML 파일에서 원하는 정보를 검색하여 이를 뽑아내는 파서(문장의 구조 분석 및 오류 점검 프로그램)이며 웹 크롤러를 만들 때 필수적인 모듈이다.

```python
import wx;
from sqlitedb import *;
from userdb import *;

data = [];


app = wx.App();
# UI Component ....
frame = wx.Frame(None, title='Shop Management');
panel1 = wx.Panel(frame);
panel2 = wx.Panel(frame);
panel1.SetBackgroundColour(colour = 'blue');
panel2.SetBackgroundColour(colour = 'green');

textId = wx.TextCtrl(panel1);
textPwd = wx.TextCtrl(panel1);
textName = wx.TextCtrl(panel1);
bt = wx.Button(panel1, label='ADD');

userList = wx.ListBox(panel2,choices=data);
userList.SetBackgroundColour(colour='white');
userList.SetSize(wx.Size(180,200));
# List Event
def itemselect(event):
    dataCnt = userList.GetSelection();
    wx.MessageBox(data[dataCnt],'User Information',wx.OK);

userList.Bind(wx.EVT_LISTBOX, itemselect);

# Button Event...
def onClick(event):
    id = textId.GetValue();
    pwd = textPwd.GetValue();
    name = textName.GetValue();
    wx.MessageBox(id, 'Alert', wx.OK);
    data.append(id+' '+name);
    userList.Append(id+' '+name);
    textId.SetValue(' ');
    textPwd.SetValue(' ');
    textName.SetValue(' ');
    #print(id,pwd,name); # DB에 입력하는 코드를 작성할 수 있다.
bt.Bind(wx.EVT_BUTTON,onClick);

box1 = wx.BoxSizer(wx.VERTICAL);
box1.Add(textId);
box1.Add(textPwd);
box1.Add(textName);
box1.Add(bt);
panel1.SetSizer(box1);

# Grid ...
grid = wx.GridSizer(1,2,10,10);
grid.Add(panel1,0,wx.EXPAND);
grid.Add(panel2,0,wx.EXPAND);

frame.SetSizer(grid);
frame.Show(True);
app.MainLoop();
```

 * pip install wxPython을 설치하면 UI를 만들 수 있다.
   	* file - setting - project - pyhton interpreter - show all - 해당 파일 - pythton(base)로 하면 모든 디렉토리에서 wxPython을 사용할 수 있다.

```python
import wx
from sqlitedb import *
from userdb import *
import sqlite3

########################################
"""
1. 화면에서 데이터를 입력하면 db에 인서트되고 리스트 화면에 출력

2. 리스트 화면에는 id와 name만 출력되고 항목을 클릭했을때
아이디 패스워드 이름 모두 출력 되게 한다.

db 연동
입력 시 db에 저장
입력값 db에서 가져와서 출력
"""

data = ['3','4','5']
app = wx.App()
# UI Component ................................

frame = wx.Frame(None,title='Shop Management')
panel1 = wx.Panel(frame)
panel2 = wx.Panel(frame)
panel1.SetBackgroundColour(colour='blue')
panel2.SetBackgroundColour(colour='green')

textId = wx.TextCtrl(panel1)
textPwd = wx.TextCtrl(panel1)
textName = wx.TextCtrl(panel1)
textAge = wx.TextCtrl(panel1)
bt = wx.Button(panel1, label='ADD')

userList = wx.ListBox(panel2,choices=data)
userList.SetBackgroundColour(colour='white')
userList.SetSize(wx.Size(180,200))

#List Event ....................................
def itemselect(event):
    dataCnt = userList.GetSelection()
    wx.MessageBox(data[dataCnt],'User Infomation', wx.OK)


userList.Bind(wx.EVT_LISTBOX,itemselect)


# Button Event .................................
def onClick(event):
    id = textId.GetValue()
    pwd = textPwd.GetValue()
    name = textName.GetValue()
    age = textAame.GetValue()
    con = sqlite3.connect('userdb.db')
    cursor = con.cursor()
    cursor.execute("""CREATE TABLE IF NOT EXISTS apptb(
                        id CHAR(10) PRIMARY KEY,
                        pwd CHAR(10),
                        name CHAR(10),
                        age NUMBER(3))""")
    cursor.execute("""INSERT INTO apptb VALUES ('%s','%s','%s',%d)""" % (id, pwd, name, age))
    con.commit()
    cursor.execute("""select * from apptb""")
    allUsers = cursor.fetchall()
    # cursor.close()
    # con.close()
    wx.MessageBox(id, 'Alert', wx.OK)
    for i in range(len(allUsers)):
        data.append(allUsers[i][0]+' '+allUsers[i][2])
        userList.Append(allUsers[i][0]+' '+allUsers[i][2])
    textId.SetValue('')
    textPwd.SetValue('')
    textName.SetValue('')
bt.Bind(wx.EVT_BUTTON, onClick) #이벤트 발생시 온클릭 시행

box1 = wx.BoxSizer(wx.VERTICAL)
box1.Add(textId)
box1.Add(textPwd)
box1.Add(textName)
box1.Add(textAge)
box1.Add(bt)
panel1.SetSizer(box1)

# Grid ..................................
grid = wx.GridSizer(1,2,10,10) #1행2열 양쪽 여백넓이
grid.Add(panel1,0,wx.EXPAND)
grid.Add(panel2,1,wx.EXPAND)

frame.SetSizer(grid)
frame.Show(True)
app.MainLoop()
```

