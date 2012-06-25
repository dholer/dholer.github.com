---
layout: post
category : MySQL
tags : [mysql,mysql command,mysql 命令]
title: mysql 常用命令
---
{% include JB/setup %}

###常用命令： 

	show databases;--显示数据库  
	use databasename; --用数据库  
	show tables;--显示表  
	create table tablename(field-name-1 fieldtype-1 modifiers,field-name-2 fieldtype-2 modifiers,....);--创建表  
	alter table tablename add new-fielname new fieldtype--为表加入新列  
	insert into tablename(fieldname-1,fieldname-2,fieldname-n)valuse(value-1,value-2,value-n)--增  
	delete from tablename where fieldname=value--删  
	update tablename set fieldname=new-value where id=1--改  
	select * from tablename--查  
	desc tablename--表定义描述  
	show create table tablename--可以查看引擎  
	alter table tablename engine=InnoDB--修改引擎  
	create table tablename(id int(11),name varchar(10) )type=INNODB--建表是设置引擎  



###登录MySQL服务器后，查看当前时间，登录的用户以及数据库的版本 

	mysql> select now(),user(),version();  
	
	+---------------------+----------------+------------------+
	| now()               | user()         | version()        |
	+---------------------+----------------+------------------+
	| 2012-06-04 12:54:02 | root@localhost | 5.1.62-community |
	+---------------------+----------------+------------------+
	1 row in set (0.20 sec)
	
	
###显示数据库列表 

	mysql> show databases;  
	
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| core               |
	| drupal7            |
	| mysql              |
	+--------------------+
	4 rows in set (0.35 sec)


###新增数据库并查看 

	mysql> create database test_db;  
		
	Query OK, 1 row affected (0.00 sec)  
  
	mysql> show databases;  
	
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| core               |
	| drupal7            |
	| mysql              |
	| test_db            |
	+--------------------+
	5 rows in set (0.02 sec)


###选择数据库  
	
	mysql> use test_db;  
	Database changed  


###查看已选择的数据库： 

	mysql> select database();  
	
	+------------+  
	| database() |  
	+------------+  
	| test_db    |  
	+------------+  
	1 row in set (0.00 sec)  


###显示当前数据库的所有数据表 

	mysql> show tables;  
	Empty set (0.00 sec)  


###新建数据表并查看 

	mysql> create table person(  
    -> id int,  
    -> name varchar(20),  
    -> sex char(1),  
    -> birth date  
    -> );
      
	Query OK, 0 rows affected (0.09 sec)  

	mysql> show tables;  
	+-------------------+  
	| Tables_in_test_db |  
	+-------------------+  
	| person            |  
	+-------------------+  
	1 row in set (0.00 sec)  


###获取表结构  

	mysql> desc person;  
	+-------+-------------+------+-----+---------+-------+  
	| Field | Type        | Null | Key | Default | Extra |  
	+-------+-------------+------+-----+---------+-------+  
	| id    | int(11)     | YES  |     | NULL    |       |  
	| name  | varchar(20) | YES  |     | NULL    |       |  
	| sex   | char(1)     | YES  |     | NULL    |       |  
	| birth | date        | YES  |     | NULL    |       |  
	+-------+-------------+------+-----+---------+-------+  
	4 rows in set (0.01 sec)  

或者 

	mysql> describe person;  
	
	+-------+-------------+------+-----+---------+-------+  
	| Field | Type        | Null | Key | Default | Extra |  
	+-------+-------------+------+-----+---------+-------+  
	| id    | int(11)     | YES  |     | NULL    |       |  
	| name  | varchar(20) | YES  |     | NULL    |       |  
	| sex   | char(1)     | YES  |     | NULL    |       |  
	| birth | date        | YES  |     | NULL    |       |  
	+-------+-------------+------+-----+---------+-------+  
	4 rows in set (0.01 sec)  


###查询表中的数据 

	mysql> select * from person;  
	
	Empty set (0.00 sec)  


###插入数据 

	mysql > insert into person(id,name,sex,birth)  
    	 -> values(1,'zhangsan','1','1990-01-08');  
    	 
	Query OK, 1 row affected (0.04 sec)  

####查询表中的数据： 

	mysql> select * from person;  
	
	+------+----------+------+------------+  
	| id   | name     | sex  | birth      |  
	+------+----------+------+------------+  
	|    1 | zhangsan | 1    | 1990-01-08 |  
	+------+----------+------+------------+  
	1 row in set (0.00 sec)  


###修改字段的类型 

	mysql> alter table person modify sex char(8);  
	
	Query OK, 1 row affected (0.17 sec)  
	Records: 1  Duplicates: 0  Warnings: 0  

查看字段描述： 

	mysql> desc person;  
	
	+-------+-------------+------+-----+---------+-------+  
	| Field | Type        | Null | Key | Default | Extra |  
	+-------+-------------+------+-----+---------+-------+  
	| id    | int(11)     | YES  |     | NULL    |       |  
	| name  | varchar(20) | YES  |     | NULL    |       |  
	| sex   | char(8)     | YES  |     | NULL    |       |  
	| birth | date        | YES  |     | NULL    |       |  
	+-------+-------------+------+-----+---------+-------+  
	4 rows in set (0.01 sec)  


###新增一个字段 

	mysql> alter table person add(address varchar(50));  
	Query OK, 1 row affected (0.27 sec)  
	Records: 1  Duplicates: 0  Warnings: 0  

###查看字段描述： 

	mysql> desc person;  
	
	+---------+-------------+------+-----+---------+-------+  
	| Field   | Type        | Null | Key | Default | Extra |  
	+---------+-------------+------+-----+---------+-------+  
	| id      | int(11)     | YES  |     | NULL    |       |  
	| name    | varchar(20) | YES  |     | NULL    |       |  
	| sex     | char(8)     | YES  |     | NULL    |       |  
	| birth   | date        | YES  |     | NULL    |       |  
	| address | varchar(50) | YES  |     | NULL    |       |  
	+---------+-------------+------+-----+---------+-------+  
	5 rows in set (0.01 sec)  


###更新字段内容 

查看修改前表的内容： 

	mysql> select * from person;  
	
	+------+----------+------+------------+---------+  
	| id   | name     | sex  | birth      | address |  
	+------+----------+------+------------+---------+  
	|    1 | zhangsan | 1    | 1990-01-08 | NULL    |  
	+------+----------+------+------------+---------+  
	1 row in set (0.00 sec)  


修改： 

	mysql> update person set name='lisi' where id=1;  
	
	Query OK, 1 row affected (0.04 sec)  
	Rows matched: 1  Changed: 1  Warnings: 0  
  
	mysql> select * from person;  
	
	+------+------+------+------------+---------+  
	| id   | name | sex  | birth      | address |  
	+------+------+------+------------+---------+  
	|    1 | lisi | 1    | 1990-01-08 | NULL    |  
	+------+------+------+------------+---------+  
	1 row in set (0.00 sec)  
	  
	mysql> update person set sex='man',address='China' where id=1;  
	
	Query OK, 1 row affected (0.04 sec)  
	Rows matched: 1  Changed: 1  Warnings: 0  
	  
	mysql> select * from person;  
	
	+------+------+------+------------+---------+  
	| id   | name | sex  | birth      | address |  
	+------+------+------+------------+---------+  
	|    1 | lisi | man  | 1990-01-08 | China   |  
	+------+------+------+------------+---------+  
	1 row in set (0.00 sec)  



为了方便下面测试删除数据，在向person表中插入2条数据： 

	mysql> insert into person(id,name,sex,birth,address)  
	    -> values(2,'wangwu','man','1990-01-10','China');  
	Query OK, 1 row affected (0.02 sec)  
	  
	mysql> insert into person(id,name,sex,birth,address)  
	    -> values(3,'zhangsan','man','1990-01-10','China');  
	Query OK, 1 row affected (0.04 sec)  
	  
	mysql> select * from person;  
	+------+----------+------+------------+---------+  
	| id   | name     | sex  | birth      | address |  
	+------+----------+------+------------+---------+  
	|    1 | lisi     | man  | 1990-01-08 | China   |  
	|    2 | wangwu   | man  | 1990-01-10 | China   |  
	|    3 | zhangsan | man  | 1990-01-10 | China   |  
	+------+----------+------+------------+---------+  
	3 rows in set (0.00 sec)  


###删除表中的数据 
删除表中指定的数据： 

	mysql> delete from person where id=2;  
	Query OK, 1 row affected (0.02 sec)  
	  
	mysql> select * from person;  
	+------+----------+------+------------+---------+  
	| id   | name     | sex  | birth      | address |  
	+------+----------+------+------------+---------+  
	|    1 | lisi     | man  | 1990-01-08 | China   |  
	|    3 | zhangsan | man  | 1990-01-10 | China   |  
	+------+----------+------+------------+---------+  
	2 rows in set (0.00 sec)  

删除表中全部的数据： 

	mysql> delete from person;  
	Query OK, 2 rows affected (0.04 sec)  
	  
	mysql> select * from person;  
	Empty set (0.00 sec)  


###重命名表 
查看重命名前的表名： 

	mysql> show tables;  
	
	+-------------------+  
	| Tables_in_test_db |  
	+-------------------+  
	| person            |  
	+-------------------+  
	1 row in set (0.00 sec)  

重命名： 

	mysql> alter table person rename person_test;  
	Query OK, 0 rows affected (0.04 sec)  
	  
	mysql> show tables;  
	+-------------------+  
	| Tables_in_test_db |  
	+-------------------+  
	| person_test       |  
	+-------------------+  
	1 row in set (0.00 sec)  


###新增主键 

	mysql> alter table person_test add primary key(id);  
	Query OK, 0 rows affected (0.11 sec)  
	Records: 0  Duplicates: 0  Warnings: 0  
	  
	mysql> desc person_test;  
	+---------+-------------+------+-----+---------+-------+  
	| Field   | Type        | Null | Key | Default | Extra |  
	+---------+-------------+------+-----+---------+-------+  
	| id      | int(11)     | NO   | PRI | 0       |       |  
	| name    | varchar(20) | YES  |     | NULL    |       |  
	| sex     | char(8)     | YES  |     | NULL    |       |  
	| birth   | date        | YES  |     | NULL    |       |  
	| address | varchar(50) | YES  |     | NULL    |       |  
	+---------+-------------+------+-----+---------+-------+  
	5 rows in set (0.00 sec)  


###删除主键： 

	mysql> alter table person_test drop primary key;  
	Query OK, 0 rows affected (0.18 sec)  
	Records: 0  Duplicates: 0  Warnings: 0  
	  
	mysql> desc person_test;  
	+---------+-------------+------+-----+---------+-------+  
	| Field   | Type        | Null | Key | Default | Extra |  
	+---------+-------------+------+-----+---------+-------+  
	| id      | int(11)     | NO   |     | 0       |       |  
	| name    | varchar(20) | YES  |     | NULL    |       |  
	| sex     | char(8)     | YES  |     | NULL    |       |  
	| birth   | date        | YES  |     | NULL    |       |  
	| address | varchar(50) | YES  |     | NULL    |       |  
	+---------+-------------+------+-----+---------+-------+  
	5 rows in set (0.01 sec)  


###删除表 

	mysql> drop table person_test;  
	Query OK, 0 rows affected (0.04 sec)  
	  
	mysql> show tables;  
	Empty set (0.00 sec)  


###删除数据库 
 
	mysql> show databases;  
	+--------------------+  
	| Database           |  
	+--------------------+  
	| information_schema |  
	| mysql              |  
	| performance_schema |  
	| test               |  
	| test_db            |  
	+--------------------+  
	5 rows in set (0.00 sec)  
	  
	mysql> drop database test_db;  
	Query OK, 0 rows affected (0.11 sec)  
	  
	mysql> show databases;  
	+--------------------+  
	| Database           |  
	+--------------------+  
	| information_schema |  
	| mysql              |  
	| performance_schema |  
	| test               |  
	+--------------------+  
	4 rows in set (0.00 sec)  


###查看建表语句 

	mysql> show create table table_name;  

