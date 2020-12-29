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
## php-mariadb
* cd /opt/docs/
* vim test.php
```
<?php
$server = "localhost";         # MySQL/MariaDB 伺服器
$dbuser = "root";       # 使用者帳號
$dbpassword = "1234"; # 使用者密碼
$dbname = "testdb";    # 資料庫名稱

# 連接 MySQL/MariaDB 資料庫
$connection = new mysqli($server, $dbuser, $dbpassword, $dbname);

# 檢查連線是否成功
if ($connection->connect_error) {
  die("連線失敗：" . $connection->connect_error);
}


$sqlQuery ="SELECT * from Persons;";

if ($result = $connection->query($sqlQuery)) {
  # 取得結果
  while ($row = $result->fetch_row()) {
    printf ("%s：%d\n", $row[0], $row[1]);
  }

  # 釋放資源
  $result->close();
} else {
  echo "執行失敗：" . $connection->error;
}

$connection->close();
?>
```
* http://192.168.23.137/data/test.php
