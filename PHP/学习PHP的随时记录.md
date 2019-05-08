# 学习并使用PHP开发网站过程中的记录
## 系统信息
### 数据库
- IP = localhost:3306 + root:123456
- 专用数据库: comdb
- 运行User = phpuser@localhost:phpuserpw

## 登陆界面
- 登陆界面中，使用js来检验并提交数据。
- 提交数据的时候使用ajax的方式。
- 验证登陆的controller只完成登陆验证、设置session的职责，通过json格式的返回值与前端沟通，不返回整个的html页面。

### 使用MD5将密码加密后传输
在将密码传递到服务器端之前，先使用MD5函数将密码进行加密。在加密的过程中，我使用的是jQuery提供的MD5函数。
需要在使用MD5功能的html页面中，增加对相关js的引用。

```html
<script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
<script type="text/javascript" src="jquery.md5.js"></script>
```

### 登陆功能的数据库设计
数据库中的用户表用户存储用户基础数据，支撑用户的登陆功能。
```markdown
数据表名称: 
  co_user. 其中的‘co_’前缀用来避免和各种系统、框架的保留字产生冲突，也有company和commerce的意思。
数据字段:
1. ID,用户主键，自增
2. user_nick_name, 用户昵称
3. user_phone,用户手机号，用于手机号登陆
4. user_pwd, 用户密码，此处的数据为用户密码与user_salt二次加密运算后的数据。例如用户的密码为123456，user_salt = 'abcd'，则user_pwd中存储的数值为MD5(MD5(123456) + MD5(abcd)).
5. user_salt, 用户将用户密码加密的字段。
6. user_register_date，用户注册时间，使用UTC-timestamp，因为整数运算方便。MYSQL里面的timestamp对应JAVA中的java.sql.Timestamp
7. user_status, 用户状态，32位整数。用于可能的用户管理功能，使用位运算的方式，可以支持32个控制标记。
```

向co_user数据表中插入数据的SQL语句如下。
```sql
INSERT INTO co_user (user_nick_name, user_phone, user_pwd, user_salt, user_register_time) 
VALUES ('root_admin', '15901559713', 'd4320fdde3f43b8a839761fb202c2298', 'd8b6719a', now())
```

# References
- [jQuery-MD5官方地址](https://github.com/placemarker/jQuery-MD5)
- [在线HTML/CSS/JavaScript代码运行工具](http://tools.jb51.net/code/HtmlJsRun)