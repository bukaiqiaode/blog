# 学习并放弃使用PHP开发网站过程中的记录
## 系统信息
### 数据库
- IP = localhost:3306 + root:123456
- 专用数据库: comdb
- 运行User = phpuser@localhost:phpuserpw

## 登陆
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
数据库中的用户表存储用户基础数据，支撑用户的登陆功能。
```markdown
数据表名称: 
  co_user. 其中的‘co_’前缀用来避免和各种系统、框架的保留字产生冲突，也有company和commerce的意思。
主要数据字段:
1. auto_id, 用户主键，自增
2. user_id, 系统内部使用的用户id，用于系统数据库移植。
3. user_phone, 用户手机号，用于手机号登陆
4. user_pwd, 用户密码，此处的数据为用户密码与user_salt二次加密运算后的数据。例如用户的密码为123456，user_salt = 'abcd'，则user_pwd中存储的数值为MD5(MD5(123456) + MD5(abcd)).
5. user_salt, 用户将用户密码加密的字段。
6. user_register_time，用户注册时间，使用UTC-timestamp，因为整数运算方便。MYSQL里面的timestamp对应JAVA中的java.sql.Timestamp
7. user_status, 用户状态，32位整数。用于可能的用户管理功能，使用位运算的方式，可以支持32个控制标记。
```

SQL相关语句如下。
```sql
-- Table [co_user] is used to store login information for users.
-- Below is the script to create the table co_user
CREATE TABLE IF NOT EXISTS `co_user` (
  `auto_id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` varchar(32) NOT NULL UNIQUE COMMENT '32位用户ID，用于系统迁移',
  `user_name` varchar(20) NOT NULL UNIQUE COMMENT '登陆用户名，不可重复',
  `user_email` varchar(32) NOT NULL UNIQUE COMMENT '登陆email，不可重复',
  `user_phone` varchar(20) NOT NULL  UNIQUE COMMENT '登陆手机号，不可重复',
  `user_pwd` varchar(32) NOT NULL,
  `user_status` int(11) NOT NULL DEFAULT '0',
  `user_register_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `user_salt` varchar(32) DEFAULT NULL,
  `user_token` varchar(64) DEFAULT NULL,
  PRIMARY KEY (`auto_id`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8mb4;

-- Below is the script to insert values into co_user table.
INSERT INTO co_user (user_id, user_name, user_email, user_phone, user_pwd, user_status, user_register_time, user_salt) 
VALUES ('0418d5a0d03411e893acfa163ed59cae', 'root_admin', '15638444@qq.com', '15901559713', 'd4320fdde3f43b8a839761fb202c2298', 0, now(), 'd8b6719a');
INSERT INTO co_user (user_id, user_name, user_email, user_phone, user_pwd, user_status, user_register_time, user_salt) 
VALUES ('0418d5a0d03411e893acfa163ed59ca1', 'nogroup1', 'nogroup1@qq.com', '15901559711', 'd4320fdde3f43b8a839761fb202c2298', 0, now(), 'd8b67191');
INSERT INTO co_user (user_id, user_name, user_email, user_phone, user_pwd, user_status, user_register_time, user_salt) 
VALUES ('0418d5a0d03411e893acfa163ed59ca2', 'nogroup2', 'nogroup2@qq.com', '15901559712', 'd4320fdde3f43b8a839761fb202c2298', 0, now(), 'd8b67191');

-- Table [co_user_group] is used to store the basic information of user-groups.
-- There are M-N relationship between users & user-groups.
-- Below is the script to create the co_user_group table.
CREATE TABLE IF NOT EXISTS `co_usergroup` (
  `auto_id` int(11) NOT NULL AUTO_INCREMENT,
  `ugroup_name` varchar(20) NOT NULL UNIQUE COMMENT '用户组的显示名称',
  `ugroup_created_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `ugroup_status` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`auto_id`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8mb4;

-- Table [co_user_group_mapping] is used to maintain the relationship between users & groups.
-- Below is the script to create the co_user_group_mapping table & foreign-key.
CREATE TABLE IF NOT EXISTS `co_user_group_mapping` (
  `auto_id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` varchar(32) NOT NULL COMMENT '用户ID',
  `ugroup_id` int(11) NOT NULL COMMENT '用户所属用户组ID',
  `mcreated` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `mstatus` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`auto_id`),
  INDEX u_id (`user_id`),
  INDEX ugroup_id (`ugroup_id`),
  FOREIGN KEY (`user_id`) REFERENCES `co_user`(`user_id`) ON DELETE CASCADE,
  FOREIGN KEY (`ugroup_id`) REFERENCES `co_usergroup`(`auto_id`) ON DELETE CASCADE
) ENGINE=InnoDB  DEFAULT CHARSET=utf8mb4;

```


## 主界面
- 用户登陆成功后将被跳转到主界面，main.html
- 主界面是一个框架界面，通过包含不同的界面来为用户提供服务，用户在登陆状态的操作都在主界面内完成。
- 主界面提供退出登陆的选项，用户退出登陆后将被跳转回登陆界面。

# References
- [jQuery-MD5官方地址](https://github.com/placemarker/jQuery-MD5)
- [在线HTML/CSS/JavaScript代码运行工具](http://tools.jb51.net/code/HtmlJsRun)