
mysql> CREATE DATABASE CollegeDB;
Query OK, 1 row affected (0.01 sec)

mysql>  USE CollegeDB;
Database changed
mysql>  CREATE TABLE Departments ( DeptID INT PRIMARY KEY, DeptName VARCHAR(100), StudentCount INT,  TeacherCount INT, Classrooms INT,  Email VARCHAR(100) );
Query OK, 0 rows affected (0.04 sec)

mysql>  START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql>  INSERT INTO Departments VALUES (1, 'Computer Science', 200, 15, 5,  'cs@college.edu');
Query OK, 1 row affected (0.02 sec)

mysql> SAVEPOINT sp1;
Query OK, 0 rows affected (0.00 sec)

mysql>  INSERT INTO Departments VALUES (2, 'Information Technology', 180,  12, 4, 'it@college.edu');
Query OK, 1 row affected (0.00 sec)

mysql>  SAVEPOINT sp2;
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT INTO Departments VALUES (3, 'Cyber Security', 150, 10, 6,  'cyber@college.edu');
Query OK, 1 row affected (0.00 sec)

mysql> SAVEPOINT sp3;
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT INTO Departments VALUES (4, 'Electronics', 150, 10, 6,  'ece@college.edu');
Query OK, 1 row affected (0.00 sec)

mysql> SELECT * FROM Departments;
+--------+------------------------+--------------+--------------+------------+-------------------+
| DeptID | DeptName               | StudentCount | TeacherCount | Classrooms | Email             |
+--------+------------------------+--------------+--------------+------------+-------------------+
|      1 | Computer Science       |          200 |           15 |          5 | cs@college.edu    |
|      2 | Information Technology |          180 |           12 |          4 | it@college.edu    |
|      3 | Cyber Security         |          150 |           10 |          6 | cyber@college.edu |
|      4 | Electronics            |          150 |           10 |          6 | ece@college.edu   |
+--------+------------------------+--------------+--------------+------------+-------------------+
4 rows in set (0.00 sec)

mysql> UPDATE Departments SET StudentCount = 220 WHERE DeptID = 1;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> UPDATE Departments SET StudentCount = 190 WHERE DeptID = 2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SAVEPOINT sp4;
Query OK, 0 rows affected (0.00 sec)

mysql> ROLLBACK TO sp2;
Query OK, 0 rows affected (0.00 sec)

mysql>  SELECT * FROM Departments;
+--------+------------------------+--------------+--------------+------------+----------------+
| DeptID | DeptName               | StudentCount | TeacherCount | Classrooms | Email          |
+--------+------------------------+--------------+--------------+------------+----------------+
|      1 | Computer Science       |          200 |           15 |          5 | cs@college.edu |
|      2 | Information Technology |          180 |           12 |          4 | it@college.edu |
+--------+------------------------+--------------+--------------+------------+----------------+
2 rows in set (0.00 sec)

mysql>  INSERT INTO Departments VALUES (5, 'Mechanical', 160, 11, 7,  'mech@college.edu');
Query OK, 1 row affected (0.00 sec)

mysql>  INSERT INTO Departments VALUES (5, 'Mechanical', 160, 11, 7,  'mech@college.edu');
ERROR 1062 (23000): Duplicate entry '5' for key 'departments.PRIMARY'
mysql> DELETE FROM Departments WHERE DeptID = 2;
Query OK, 1 row affected (0.01 sec)

mysql>  COMMIT;
Query OK, 0 rows affected (0.00 sec)

mysql> CREATE USER 'dept_user'@'localhost' IDENTIFIED BY 'pass123';
Query OK, 0 rows affected (0.04 sec)

mysql> GRANT SELECT, INSERT ON CollegeDB.Departments TO  'dept_user'@'localhost';
Query OK, 0 rows affected (0.00 sec)

mysql> mysql -u dept_user -pmysql
    -> USE CollegeDB;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'mysql -u dept_user -pmysql
USE CollegeDB' at line 1
mysql> SELECT * FROM Departments;
+--------+------------------+--------------+--------------+------------+------------------+
| DeptID | DeptName         | StudentCount | TeacherCount | Classrooms | Email            |
+--------+------------------+--------------+--------------+------------+------------------+
|      1 | Computer Science |          200 |           15 |          5 | cs@college.edu   |
|      5 | Mechanical       |          160 |           11 |          7 | mech@college.edu |
+--------+------------------+--------------+--------------+------------+------------------+
2 rows in set (0.00 sec)

mysql> INSERT INTO Departments VALUES (6, 'Artificial Intelligence', 140, 9,  4, 'ai@college.edu');
Query OK, 1 row affected (0.00 sec)

mysql> DELETE FROM Departments WHERE DeptID = 1;
Query OK, 1 row affected (0.34 sec)

mysql>  -- ERROR 1142 (42000): DELETE command denied to user 'dept_user'@'localhost'
mysql> GRANT DELETE, UPDATE ON CollegeDB.Departments TO  'dept_user'@'localhost';
Query OK, 0 rows affected (0.37 sec)

mysql>  REVOKE DELETE, INSERT, UPDATE ON CollegeDB.Departments  FROM 'dept_user'@'localhost';
Query OK, 0 rows affected (0.04 sec)

mysql> DROP USER 'dept_user'@'localhost';
Query OK, 0 rows affected (0.11 sec)
