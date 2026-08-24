mysql> drop database company;
Query OK, 2 rows affected (0.05 sec)

mysql> create database company;
Query OK, 1 row affected (0.02 sec)

mysql> use company;
Database changed
mysql> create table employee (emp_no int primary key,e_name varchar(50),e_address varchar (100),e_ph_no varchar (15),dept_no int, dept_name varchar (50), job_id char (100, salary decimal (10,2));
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ', salary decimal (10,2))' at line 1
mysql> create table employee (emp_no int primary key,e_name varchar(50),e_address varchar (100),e_ph_no varchar (15),dept_no int, dept_name varchar (50), job_id char (10), salary decimal (10,2));
Query OK, 0 rows affected (0.05 sec)

mysql> describe employee;
+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| emp_no    | int           | NO   | PRI | NULL    |       |
| e_name    | varchar(50)   | YES  |     | NULL    |       |
| e_address | varchar(100)  | YES  |     | NULL    |       |
| e_ph_no   | varchar(15)   | YES  |     | NULL    |       |
| dept_no   | int           | YES  |     | NULL    |       |
| dept_name | varchar(50)   | YES  |     | NULL    |       |
| job_id    | char(10)      | YES  |     | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
8 rows in set (0.01 sec)

mysql> alter table employee add hiredate date;
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table employee modify job_id varchar(10);
Query OK, 0 rows affected (0.10 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table employee rename column emp_no to e_no;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table employee modify job_id varchar (20);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table employee add constraint uq_e_ph_no unique (e_ph_no);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table employee modify_name varchar (5) not null;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'modify_name varchar (5) not null' at line 1
mysql> alter table employee modify_name varchar (50) not null;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'modify_name varchar (50) not null' at line 1
mysql> alter table employee modify e_name varchar(50) not null;
Query OK, 0 rows affected (0.08 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table employee add constraint chk_salary check(salary>0);
Query OK, 0 rows affected (0.10 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> insert into employee (e_no, e_name, e_address, e_ph_no,dept_no,dept_name,job_id,salary,hiredate)values(1,'john doe', '123 main st', '555-1234', 101, 'sales', 'j1001', 50000.00, ('20-08-24');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' at line 1
mysql> insert into employee (e_no, e_name, e_address, e_ph_no,dept_no,dept_name,job_id,salary,hiredate)values(1,'john doe', '123 main st', '555-1234', 101, 'sales', 'j1001', 50000.00,('20-08-24');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' at line 1
mysql> insert into employee (e_no, e_name, e_address, e_ph_no, dept_no, dept_name, job_id, salary, hiredate)values(1,'blue','163 main st', '666-2325', 202, 'sales','b2001', 50000.00,'20-08-24');
Query OK, 1 row affected (0.01 sec)

mysql> insert into employee (e_no, e_name, e_address, e_ph_no, dept_no, dept_name, job_id, salary, hiredate)values (2, 'jane smith', '456 oak st', '555-5678', 102, 'marketing', 'j1002', 60000.00, '4-06-18');
Query OK, 1 row affected (0.01 sec)

mysql> insert into employee (e_no, e_name, e_address, e_ph_no, dept_no, dept_name, job_id, salary, hiredate)values(3, 'alice joesh', '789 pine st', '555-9012', 103, 'hr', 'j1003', 55000.00, '4-07-15');
Query OK, 1 row affected (0.01 sec)

mysql> insert into employee (e_no, e_name, e_address, e_ph_no, dept_no, dept_name, job_id, salary, hiredate)values(4, 'baby', '112 apple st', '555-1123', 103, 'admin', 'j1004', 34000.00, '4-03-15');
Query OK, 1 row affected (0.01 sec)

mysql> select * from employee;
+------+-------------+--------------+----------+---------+-----------+--------+----------+------------+
| e_no | e_name      | e_address    | e_ph_no  | dept_no | dept_name | job_id | salary   | hiredate   |
+------+-------------+--------------+----------+---------+-----------+--------+----------+------------+
|    1 | blue        | 163 main st  | 666-2325 |     202 | sales     | b2001  | 50000.00 | 2020-08-24 |
|    2 | jane smith  | 456 oak st   | 555-5678 |     102 | marketing | j1002  | 60000.00 | 0004-06-18 |
|    3 | alice joesh | 789 pine st  | 555-9012 |     103 | hr        | j1003  | 55000.00 | 0004-07-15 |
|    4 | baby        | 112 apple st | 555-1123 |     103 | admin     | j1004  | 34000.00 | 0004-03-15 |
+------+-------------+--------------+----------+---------+-----------+--------+----------+------------+
4 rows in set (0.00 sec)

mysql> update employee set salary=55000.00 where e_no=1;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update employee set dept_name= 'digital marketing' where dept_no=102;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from employee;
+------+-------------+--------------+----------+---------+-------------------+--------+----------+------------+
| e_no | e_name      | e_address    | e_ph_no  | dept_no | dept_name         | job_id | salary   | hiredate   |
+------+-------------+--------------+----------+---------+-------------------+--------+----------+------------+
|    1 | blue        | 163 main st  | 666-2325 |     202 | sales             | b2001  | 55000.00 | 2020-08-24 |
|    2 | jane smith  | 456 oak st   | 555-5678 |     102 | digital marketing | j1002  | 60000.00 | 0004-06-18 |
|    3 | alice joesh | 789 pine st  | 555-9012 |     103 | hr                | j1003  | 55000.00 | 0004-07-15 |
|    4 | baby        | 112 apple st | 555-1123 |     103 | admin             | j1004  | 34000.00 | 0004-03-15 |
+------+-------------+--------------+----------+---------+-------------------+--------+----------+------------+
4 rows in set (0.00 sec)

mysql> delete from employee where e_no=3;
Query OK, 1 row affected (0.02 sec)

mysql> delete from employee where dept_no=102;
Query OK, 1 row affected (0.01 sec)

mysql> select * from employee;
+------+--------+--------------+----------+---------+-----------+--------+----------+------------+
| e_no | e_name | e_address    | e_ph_no  | dept_no | dept_name | job_id | salary   | hiredate   |
+------+--------+--------------+----------+---------+-----------+--------+----------+------------+
|    1 | blue   | 163 main st  | 666-2325 |     202 | sales     | b2001  | 55000.00 | 2020-08-24 |
|    4 | baby   | 112 apple st | 555-1123 |     103 | admin     | j1004  | 34000.00 | 0004-03-15 |
+------+--------+--------------+----------+---------+-----------+--------+----------+------------+
2 rows in set (0.01 sec)

mysql> truncate table employee;
Query OK, 0 rows affected (0.06 sec)

mysql> select*from employee;
Empty set (0.00 sec)
