# pymysql

## 基本使用

```sql
# 数据库中有的数据
+----+------+-----+
| id | user | pwd |
+----+------+-----+
|  1 | shen | 111 |
+----+------+-----+
```



```python
import pymysql
user=input('请输入账号').strip()
pwd=input('请输入密码').strip()
# 建立连接
conn=pymysql.connect(
    # 本机就写host，远程连接，就写ip地址。
    host='localhost',
    port=3306,
    user='root',
    password='123456',
    db='db2',
    # 数据库就是utf8,没有utf-8，
    charset='utf8',
)
# 拿到游标    游标就是在cmd中的光标。光标等待你的输入sql语句。  游标可以是输入sql语句的接口。
cursor=conn.cursor()
# 执行sql语句。%s 是一个占位符，等待传值。
# 这条语句是判断输入的数据在数据库中是否存在。
sql='select * from userinfo where user=  "%s" and pwd="%s"'%(user,pwd)
# rows 是受影响的行数。  是int类型。
rows=cursor.execute(sql)
# 关闭游标，
cursor.close()
# 关闭数据库
conn.close()
#  进行判断
if rows:
    print('登录成功')
else:
    print('登录失败')
```

## sql注入问题

```sql
沈泽昊" -- szcs
dxxx" or 1=1 -- asdwazcs
输入这两个也可以不输入密码，也会登录成功。这种情况必须有解决方法
第一种。
	有好多的网站在我们注册账号时，不允许有特殊符号。
第二种
    修改代码。看下面的代码
```

这样可以解决sql注入的问题，建议这样写。

```sql
import pymysql
user=input('请输入账号').strip()
pwd=input('请输入密码').strip()
# 建立连接
conn=pymysql.connect(
    # 本机就写host，远程连接，就写ip地址。
    host='localhost',
    port=3306,
    user='root',
    password='123456',
    db='db2',
    # 数据库就是utf8,没有utf-8，
    charset='utf8',
)
# 拿到游标    游标就是在cmd中的光标。光标等待你的输入sql语句。  游标可以是输入sql语句的接口。
cursor=conn.cursor()
# 执行sql语句。%s 是一个占位符，等待传值。
# 这条语句是判断输入的数据在数据库中是否存在。
sql='select * from userinfo where user = %s and pwd=%s'
rows=cursor.execute(sql,(user,pwd))
# 关闭游标，
cursor.close()
# 关闭数据库
conn.close()
#  进行判断
if rows:
    print('登录成功')
else:
    print('登录失败')
```

## 增删改查操作

### 增

```python
import pymysql
# 建立连接
conn=pymysql.connect(
    host='localhost',
    port=3306,
    user='root',
    password='123456',
    db='db2',
    charset='utf8',
)
cursor=conn.cursor()
sql='insert into userinfo(user,pwd) values(%s,%s);'
# 插入一条数据
rows=cursor.execute(sql,(rrr,111))
# 插入多条数据。 也可以用for循环。
rows=cursor.executemany(sql,[("qqq",111),("www",222),("eeee",111)])
print(cursor.lastrowid) # 可以查看从m一天开始插入的
print(rows)
conn.commit()
# 关闭游标，
cursor.close()
# 关闭数据库
conn.close()
```

### 查

```python
import pymysql
# 建立连接
conn=pymysql.connect(
    host='localhost',
    port=3306,
    user='root',
    password='123456',
    db='db2',
    # 数据库就是utf8,没有utf-8，
    charset='utf8',
)
# 拿到游标    游标就是在cmd中的光标。光标等待你的输入sql语句。  游标可以是输入sql语句的接口。
cursor=conn.cursor()
# 输出的内容以字典的形式{字段：值}
# cursor=conn.cursor(pymysql.cursors.DictCursor)
sql='select * from userinfo;'
# 将sql语句传给游标。
cursor.execute(sql)
# 想要拿到数据库中的前两个数据。
print(cursor.fetchmany(2))
# 拿到数据数据库的第一条数据，依次向下
print(cursor.fetchone())
# 拿到数据库的全部数据。
print(cursor.fetchall())
conn.commit()
# 关闭游标，
cursor.close()
# 关闭数据库
conn.close()
```

#### 光标移动

```python
import pymysql
# 建立连接
conn=pymysql.connect(
    host='localhost',
    port=3306,
    user='root',
    password='123456',
    db='db2',
    # 数据库就是utf8,没有utf-8，
    charset='utf8',
)
cursor=conn.cursor()
# 输出的内容以字典的形式{字段：值}
# cursor=conn.cursor(pymysql.cursors.DictCursor)
sql='select * from userinfo;'
cursor.execute(sql)
 # 相对绝对位置移动 从表的第一个数据开始向后移动三个数据。
cursor.scroll(3,mode='absolute')   
# 拿到数据数据库的第一条数据，依次向下
print(cursor.fetchone())
# 相对当前位置移动 已经取了一个，在这个之后移动两个数据，
cursor.scroll(2,mode='relative') 
print(cursor.fetchone())
# 关闭游标，
cursor.close()
# 关闭数据库
conn.close()
```

