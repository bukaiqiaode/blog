# 本文记录Python向MySql数据库插入中文数据时的坑，并填坑

## 前提
- 你有一个MySql数据库
- 你知道怎么使用Connector/Python去连接到这个数据库，如果不知道请去官方网站学习，链接在文末引用部分。

## Python版本
```
  Python 2.7.13, on 64-bit Windows 10
```

## 从零开始
老样子，建立一个最简单的脚本，确保没有错在第一行代码
```python
      # -*- coding: UTF-8 -*-
      
      if __name__ == "__main__":
          print "Done"
```

- 图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片


## 接下来，引入`mysql.connector`的包，并定义连接到MySQL服务器需要使用的参数
```python
      # -*- coding: UTF-8 -*-
      import mysql.connector
      from mysql.connector import errorcode

      config = {
          'user' : 'xxxx',
          'password' : 'xxxx',
          'host' : 'xxxx',
          'raise_on_warnings' : True,
          }

      if __name__ == "__main__":
          print "Done"
```

## 建立一个数据库
我们希望建立一个新的数据库来做测试，就叫它`testdb`吧
```python
      # -*- coding: UTF-8 -*-
      import mysql.connector
      from mysql.connector import errorcode

      config = {
          'user' : 'xxxx',
          'password' : 'xxxx',
          'host' : 'xxxx',
          'raise_on_warnings' : True,
          }

      DB_NAME = 'testdb'
      
      if __name__ == "__main__":
          print "Done"
```

使用Connector/Python提供的功能，连接一下数据库看看
```python
      # -*- coding: UTF-8 -*-
      import mysql.connector
      from mysql.connector import errorcode

      config = {
          'user' : 'xxx',
          'password' : 'xxx',
          'host' : 'xxx',
          'raise_on_warnings' : True,
          }

      DB_NAME = 'testdb'

      if __name__ == "__main__":
          #connect to the server, doesn't specify the database to use
          cnx = mysql.connector.connect(**config)

          #get the cursor to use
          cursor = cnx.cursor()

          try:
              #switch to use the database
              cnx.database = DB_NAME
          except mysql.connector.Error as err:
              print err

          #close the cursor
          cursor.close()
          cnx.close()

          print "Done"
```
- 图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片

运行后，出现错误，提示数据库不存在。这很正常，这个数据库本来就不该存在。<br/>
添加创建数据库的函数，调用它来创建数据库。创建数据库时，指定使用 **utf-8** 编码。
```python
      # -*- coding: UTF-8 -*-
      import mysql.connector
      from mysql.connector import errorcode

      config = {
          'user' : 'xxx',
          'password' : 'xxx',
          'host' : 'xxx',
          'raise_on_warnings' : True,
          }

      DB_NAME = 'testdb'

      def create_database(cursor):
          '''
              Use the cursor to create the database
          '''
          try:
              cursor.execute(
                  "create database {} default character set 'utf8'".format(DB_NAME)
                  )
          except mysql.connector.Error as err:
              print "failed creating database:{}".format(err)
              exit(1)

      if __name__ == "__main__":
          #connect to the server, doesn't specify the database to use
          cnx = mysql.connector.connect(**config)

          #get the cursor to use
          cursor = cnx.cursor()

          try:
              #switch to use the database
              cnx.database = DB_NAME
          except mysql.connector.Error as err:
              if err.errno == errorcode.ER_BAD_DB_ERROR:
                  #if we failed to switch to the database
                  #which means that it doesn't exist
                  #then we create it.
                  print "database not there, creating it..."
                  create_database(cursor)
                  cnx.database = DB_NAME
              else:
                  #some other error, just print it
                  print(err)
                  cursor.close()
                  cnx.close()
                  exit(1)

          #close the cursor
          cursor.close()
          cnx.close()

          print "Done"
```
- 图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片图片

运行成功，数据库建立了~

## 创建数据表
添加一个函数，创建一个数据表。
```python
      # -*- coding: UTF-8 -*-
      import mysql.connector
      from mysql.connector import errorcode

      config = {
          'user' : 'xxx',
          'password' : 'xxx',
          'host' : 'xxx',
          'raise_on_warnings' : True,
          }

      DB_NAME = 'testdb'

      def create_database(cursor):
          '''
              Use the cursor to create the database
          '''
          try:
              cursor.execute(
                  "create database {} default character set 'utf8'".format(DB_NAME)
                  )
          except mysql.connector.Error as err:
              print "failed creating database:{}".format(err)
              exit(1)

      def create_table_records(cursor):
          '''
              Use the cursor to create the table
          '''
          TABLES = {}
          TABLES['records'] = (
              "CREATE TABLE `records` ("
              "  `record_id` int(11) NOT NULL AUTO_INCREMENT,"
              "  `record_name` varchar(14) NOT NULL,"
              "  `created_time` TIMESTAMP default CURRENT_TIMESTAMP,"
              "  PRIMARY KEY (`record_id`)"
              ") ENGINE=InnoDB DEFAULT CHARSET=utf8")
          try:
              cursor.execute(TABLES['records'])
          except mysql.connector.Error as err:
              print "failed creating table records:{}".format(err)
              exit(1)

      if __name__ == "__main__":
          #connect to the server, doesn't specify the database to use
          cnx = mysql.connector.connect(**config)

          #get the cursor to use
          cursor = cnx.cursor()

          try:
              #switch to use the database
              cnx.database = DB_NAME
          except mysql.connector.Error as err:
              if err.errno == errorcode.ER_BAD_DB_ERROR:
                  #if we failed to switch to the database
                  #which means that it doesn't exist
                  #then we create it.
                  print "database not there, creating it..."
                  create_database(cursor)
                  cnx.database = DB_NAME
              else:
                  #some other error, just print it
                  print(err)
                  cursor.close()
                  cnx.close()
                  exit(1)

          create_table_records(cursor)
          #close the cursor
          cursor.close()
          cnx.close()

          print "Done"
```

- 运行无错
- 在服务器端，查看数据库的内容，建立成功

## 向数据表中插入一条不带中文的记录
```python
      # -*- coding: UTF-8 -*-
      import mysql.connector
      from mysql.connector import errorcode

      config = {
          'user' : 'xxx',
          'password' : 'xxx',
          'host' : 'xxx',
          'raise_on_warnings' : True,
          }

      DB_NAME = 'testdb'

      def create_database(cursor):
          '''
              Use the cursor to create the database
          '''
          try:
              cursor.execute(
                  "create database {} default character set 'utf8'".format(DB_NAME)
                  )
          except mysql.connector.Error as err:
              print "failed creating database:{}".format(err)
              exit(1)

      def create_table_records(cursor):
          '''
              Use the cursor to create the table
          '''
          TABLES = {}
          TABLES['records'] = (
              "CREATE TABLE `records` ("
              "  `record_id` int(11) NOT NULL AUTO_INCREMENT,"
              "  `record_name` varchar(14) NOT NULL,"
              "  `created_time` TIMESTAMP default CURRENT_TIMESTAMP,"
              "  PRIMARY KEY (`record_id`)"
              ") ENGINE=InnoDB DEFAULT CHARSET=utf8")
          try:
              cursor.execute(TABLES['records'])
          except mysql.connector.Error as err:
              print "failed creating table records:{}".format(err)
              exit(1)

      def insert_record(cursor):
          add_rec = "insert into records(record_name) values( 'token' )"

          str_1 = "xxxxx record"
          cmd_str = add_rec.replace('token', str_1)

          cursor.execute(cmd_str)
          cnx.commit()

      if __name__ == "__main__":
          #connect to the server, doesn't specify the database to use
          cnx = mysql.connector.connect(**config)

          #get the cursor to use
          cursor = cnx.cursor()

          try:
              #switch to use the database
              cnx.database = DB_NAME
          except mysql.connector.Error as err:
              if err.errno == errorcode.ER_BAD_DB_ERROR:
                  #if we failed to switch to the database
                  #which means that it doesn't exist
                  #then we create it.
                  print "database not there, creating it..."
                  create_database(cursor)
                  cnx.database = DB_NAME
              else:
                  #some other error, just print it
                  print(err)
                  cursor.close()
                  cnx.close()
                  exit(1)

          #create_table_records(cursor)
          insert_record(cursor)
          #close the cursor
          cursor.close()
          cnx.close()

          print "Done"
```
- 插入成功
- 查看数据库，数据在了。

## 插入一条带中文字符的数据
```python
      # -*- coding: UTF-8 -*-
      import mysql.connector
      from mysql.connector import errorcode

      config = {
          'user' : 'xxx',
          'password' : 'xxx',
          'host' : 'xxx',
          'raise_on_warnings' : True,
          }

      DB_NAME = 'testdb'

      def create_database(cursor):
          '''
              Use the cursor to create the database
          '''
          try:
              cursor.execute(
                  "create database {} default character set 'utf8'".format(DB_NAME)
                  )
          except mysql.connector.Error as err:
              print "failed creating database:{}".format(err)
              exit(1)

      def create_table_records(cursor):
          '''
              Use the cursor to create the table
          '''
          TABLES = {}
          TABLES['records'] = (
              "CREATE TABLE `records` ("
              "  `record_id` int(11) NOT NULL AUTO_INCREMENT,"
              "  `record_name` varchar(14) NOT NULL,"
              "  `created_time` TIMESTAMP default CURRENT_TIMESTAMP,"
              "  PRIMARY KEY (`record_id`)"
              ") ENGINE=InnoDB DEFAULT CHARSET=utf8")
          try:
              cursor.execute(TABLES['records'])
          except mysql.connector.Error as err:
              print "failed creating table records:{}".format(err)
              exit(1)

      def insert_record(cursor):
          add_rec = "insert into records(record_name) values( 'token' )"

          str_1 = "采育, 十里河"
          cmd_str = add_rec.replace('token', str_1)

          cursor.execute(cmd_str)
          cnx.commit()

      if __name__ == "__main__":
          #connect to the server, doesn't specify the database to use
          cnx = mysql.connector.connect(**config)

          #get the cursor to use
          cursor = cnx.cursor()

          try:
              #switch to use the database
              cnx.database = DB_NAME
          except mysql.connector.Error as err:
              if err.errno == errorcode.ER_BAD_DB_ERROR:
                  #if we failed to switch to the database
                  #which means that it doesn't exist
                  #then we create it.
                  print "database not there, creating it..."
                  create_database(cursor)
                  cnx.database = DB_NAME
              else:
                  #some other error, just print it
                  print(err)
                  cursor.close()
                  cnx.close()
                  exit(1)

          #create_table_records(cursor)
          insert_record(cursor)
          #close the cursor
          cursor.close()
          cnx.close()

          print "Done"
```

运行起来，果然出错了！

## 怎么破？请看本系列的下一篇~~~
