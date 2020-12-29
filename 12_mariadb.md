# [Mariadb](https://blog.gtwang.org/linux/centos-7-install-mariadb-mysql-server-tutorial/)
* yum install mariadb-server
* systemctl start mariadb
* mysql -u root -p
* create database testdb;
* create user 'testuser'@'localhost' identified by 'password';
* grant all on testdb.* to 'testuser'@'localhost';
* exit
* mysql -u testuser -p
* use testdb;
* create table Persons(name char(11), age int(10), city varchar(255));
```
MariaDB [testdb]> insert into Persons(name, age, city) values ("peter", 35, "Kaohsiung");
Query OK, 1 row affected (0.00 sec)

MariaDB [testdb]> insert into Persons(name, age, city) values ("mary", 32, "Kinmen");  Query OK, 1 row affected (0.01 sec)

MariaDB [testdb]> insert into Persons(name, age, city) values ("tom", 30, "Taipei");
Query OK, 1 row affected (0.00 sec)

MariaDB [testdb]> select * from Persons;
+-------+------+-----------+
| name  | age  | city      |
+-------+------+-----------+
| peter |   35 | Kaohsiung |
| mary  |   32 | Kinmen    |
| tom   |   30 | Taipei    |
+-------+------+-----------+
3 rows in set (0.00 sec)
```
