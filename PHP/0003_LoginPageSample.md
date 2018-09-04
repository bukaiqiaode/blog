# Create table in database
To make the sample simple, the table only containts the basic informaiton needed for log into the system.

```sql
CREATE TABLE IF NOT EXISTS `think_user` (
  `user_id` int(11) NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `user_name` varchar(60) UNIQUE NOT NULL,
  `user_tel` varchar(30) UNIQUE NOT NULL,
  `user_email` varchar(60) UNIQUE DEFAULT NULL,
  `user_password` varchar(255) NOT NULL,
  `user_join_time` timestamp NOT NULL DEFAULT 0,
  `user_signature` varchar(255) DEFAULT NULL,
  `user_status` int(11) NOT NULL DEFAULT 0,
  `last_modified_time` timestamp NOT NULL DEFAULT NOW() ON UPDATE NOW()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

delimiter $$
CREATE TRIGGER think_user_trigger
  before INSERT ON think_user
  FOR EACH ROW 
  BEGIN
   SET new.user_join_time = NOW();
  END;
 $$
delimiter ;
```
- A user could log-in with (`user_name`, `user_tel`, `user_email`), so these columns should be defined as `unique`. In MySQL, a `unique` column could have value `NULL`. But if its value is not `NULL`, the value must be unique.

- To make the site more safe and be in accordance with external policies, a user should provide `password` & `phone-number` as the basic information when registering. So we need to define columns (`user_name`, `user_tel`, `user_password`) as `NOT NULL`.

- `user_join_time` records the time when the user join the site, here we use a trigger to initial its value when inserting a new record. We can also define another trigger to prevent it from being changed.

- `user_status` records the status of the user, such as 'blocked' or 'deleted'. We can use bits of the column to identify different aspects.

- `last_modified_time` should be set each time the record is updated.

One sample SQL to insert a record may be:
```sql
insert into think_user(user_name, user_tel, user_password)
values('username1', '13811112222', MD5('1234abcd'));
```

# References
- [ThinkPHP5开发(一)实现登录功能](https://blog.csdn.net/u012995856/article/details/51842480)
- [MYSQL 的 primary key 和unique key 的区别](https://blog.csdn.net/yaoxy/article/details/4353115)
- [MySQL系统时间函数NOW(),CURRENT_TIMESTAMP(),SYSDATE()的区别](https://www.cnblogs.com/drcoding/p/4624851.html)
- [解决mysql的timestamp的only one current_timestamp限制](https://blog.csdn.net/wp270280522/article/details/40183175)