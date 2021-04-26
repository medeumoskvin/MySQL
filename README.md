#1)Установите СУБД MySQL. Создайте в домашней директории файл .my.cnf, задав в нем логин и пароль, который указывался при установке.
 ~ % mysql -u root -p
 ~ % nano .my.cnf
[client]
user=root
password=*********
^O

#2)Создайте базу данных example, разместите в ней таблицу users, состоящую из двух столбцов, числового id и строкового name.
~ % mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 36
Server version: 8.0.23 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE example;
Query OK, 1 row affected (0,01 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| example            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0,00 sec)

mysql> USE example;
Database changed
mysql> CREATE TABLE users (id SERIAL, name VARCHAR(100) NOT NULL UNIQUE);
Query OK, 0 rows affected (0,06 sec)

mysql> SHOW TABLES;
+-------------------+
| Tables_in_example |
+-------------------+
| users             |
+-------------------+
1 row in set (0,00 sec)


#3)Создайте дамп базы данных example из предыдущего задания, разверните содержимое дампа в новую базу данных sample.
mysql> CREATE DATABASE sample;
Query OK, 1 row affected (0,01 sec)
~ % mysqldump example > example.sql
~ % ls
<meta charset="utf-8">		PycharmProjects
Applications (Parallels)	Untitled.ipynb
Creative Cloud Files		VirtualBox VMs
Desktop				example.sql
Documents			get-pip.py
Downloads			iCloud Drive (архив)
Library				iCloud Drive (архив) - 1
Movies				index.php
Music				project
Parallels			puzzle.php
Pictures			pyTelegramBotAPI
Public
~ % mysql sample < example.sql
~ % mysql
mysql> USE sample;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+------------------+
| Tables_in_sample |
+------------------+
| users            |
+------------------+
1 row in set (0,00 sec)

#4)(по желанию) Ознакомьтесь более подробно с документацией утилиты mysqldump. Создайте дамп единственной таблицы help_keyword базы данных mysql. Причем добейтесь того, чтобы дамп содержал только первые 100 строк таблицы.
~ % mysqldump mysql help_keyword --where="TRUE ORDERBY help_keyword_id LIMIT 100"
-- MySQL dump 10.13  Distrib 8.0.23, for osx10.16 (x86_64)
--
-- Host: localhost    Database: mysql
-- ------------------------------------------------------
-- Server version	8.0.23

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!50503 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

--
-- Table structure for table `help_keyword`
--

DROP TABLE IF EXISTS `help_keyword`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `help_keyword` (
  `help_keyword_id` int unsigned NOT NULL,
  `name` char(64) NOT NULL,
  PRIMARY KEY (`help_keyword_id`),
  UNIQUE KEY `name` (`name`)
) /*!50100 TABLESPACE `mysql` */ ENGINE=InnoDB DEFAULT CHARSET=utf8 STATS_PERSISTENT=0 ROW_FORMAT=DYNAMIC COMMENT='help keywords';
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `help_keyword`
--
-- WHERE:  TRUE ORDERBY help_keyword_id LIMIT 100

mysqldump: Couldn't execute 'SELECT /*!40001 SQL_NO_CACHE */ * FROM `help_keyword` WHERE TRUE ORDERBY help_keyword_id LIMIT 100': You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'ORDERBY help_keyword_id LIMIT 100' at line 1 (1064)
macbook@MacBook-Pro-Macbook ~ % mysqldump mysql help_keyword --where="TRUE ORDERBY help_keyword_id LIMIT 100" > help_keyword_report.sql
mysqldump: Couldn't execute 'SELECT /*!40001 SQL_NO_CACHE */ * FROM `help_keyword` WHERE TRUE ORDERBY help_keyword_id LIMIT 100': You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'ORDERBY help_keyword_id LIMIT 100' at line 1 (1064)
macbook@MacBook-Pro-Macbook ~ % 

