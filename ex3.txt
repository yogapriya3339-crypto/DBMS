mysql> Create Database
    -> CREATE DATABASE CyberDB;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'CREATE DATABASE CyberDB' at line 2
mysql> 
mysql> USE CyberDB;
Database changed
mysql> 
mysql> -- 2. Create Users Table
mysql> CREATE TABLE Users (
    ->     UserID INT PRIMARY KEY,
    ->     FirstName VARCHAR(50),
    ->     LastName VARCHAR(50),
    ->     Email VARCHAR(100),
    ->     DateOfBirth DATE
    -> );
Query OK, 0 rows affected (0.04 sec)

mysql> 
mysql> -- 3. Create Enrollments Table
mysql> CREATE TABLE Enrollments (
    ->     EnrollmentID INT PRIMARY KEY AUTO_INCREMENT,
    ->     CourseTitle VARCHAR(100),
    ->     Trainer VARCHAR(50),
    ->     UserID INT,
    ->     FOREIGN KEY (UserID) REFERENCES Users(UserID)
    -> );
Query OK, 0 rows affected (0.05 sec)

mysql> 
mysql> -- 4. Insert Data into Users
mysql> INSERT INTO Users VALUES
    -> (101, 'Alice', 'Walker', 'alice.walker@example.com', '2003-06-01'),
    -> (102, 'Jack', 'Smith', 'jack.smith@example.com', '2004-07-15'),
    -> (103, 'Jenny', 'Brown', 'jenny.brown@example.com', '2005-08-20'),
    -> (104, 'David', 'Lee', 'david.lee@example.com', '2002-09-25'),
    -> (105, 'Julia', 'White', 'julia.white@example.com', '2001-10-30');
Query OK, 5 rows affected (0.02 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> 
mysql> -- 5. Insert Data into Enrollments
mysql> INSERT INTO Enrollments (CourseTitle, Trainer, UserID) VALUES
    -> ('Cyber Security Basics', 'Dr. Kevin Mitnick', 101),
    -> ('Advanced Hacking', 'Dr. Ada Lovelace', 102),
    -> ('Digital Forensics', 'Dr. Grace Hopper', 103),
    -> ('Cryptography', 'Dr. Alan Turing', 104),
    -> ('AI in Security', 'Dr. Elon Musk', 102),
    -> ('Ethical Hacking', 'Dr. Linus Torvalds', 105);
Query OK, 6 rows affected (0.01 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> 
mysql> -- 6. Count Users Born After a Certain Date
mysql> SELECT COUNT(*) AS UsersAfter2004
    -> FROM Users
    -> WHERE DateOfBirth > '2004-01-01';
+----------------+
| UsersAfter2004 |
+----------------+
|              2 |
+----------------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 7. Average EnrollmentID for Courses by Specific Trainer
mysql> SELECT AVG(EnrollmentID) AS AvgEnrollmentID
    -> FROM Enrollments
    -> WHERE Trainer = 'Dr. Alan Turing';
+-----------------+
| AvgEnrollmentID |
+-----------------+
|          4.0000 |
+-----------------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 8. Sum EnrollmentIDs for a Specific Trainer
mysql> SELECT SUM(EnrollmentID) AS TotalEnrollment
    -> FROM Enrollments
    -> WHERE Trainer = 'Dr. Grace Hopper';
+-----------------+
| TotalEnrollment |
+-----------------+
|               3 |
+-----------------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 9. Count Courses Each User is Enrolled In
mysql> SELECT UserID, COUNT(*) AS CourseCount
    -> FROM Enrollments
    -> GROUP BY UserID;
+--------+-------------+
| UserID | CourseCount |
+--------+-------------+
|    101 |           1 |
|    102 |           2 |
|    103 |           1 |
|    104 |           1 |
|    105 |           1 |
+--------+-------------+
5 rows in set (0.00 sec)

mysql> 
mysql> -- 10. Earliest DOB Among Users with Last Name 'Smith'
mysql> SELECT MIN(DateOfBirth) AS EarliestDOB
    -> FROM Users
    -> WHERE LastName = 'Smith';
+-------------+
| EarliestDOB |
+-------------+
| 2004-07-15  |
+-------------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 11. Count Courses for Users Born Before a Date
mysql> SELECT COUNT(*) AS TotalCourses
    -> FROM Enrollments
    -> WHERE UserID IN (
    ->     SELECT UserID
    ->     FROM Users
    ->     WHERE DateOfBirth < '2005-01-01'
    -> );
+--------------+
| TotalCourses |
+--------------+
|            5 |
+--------------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 12. Average UserID for a Specific Course
mysql> SELECT AVG(UserID) AS AvgUserID
    -> FROM Enrollments
    -> WHERE CourseTitle = 'Cyber Security Basics';
+-----------+
| AvgUserID |
+-----------+
|  101.0000 |
+-----------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 13. Count Courses by Each Trainer for Users Born After 2003
mysql> SELECT Trainer, COUNT(*) AS CoursesTaught
    -> FROM Enrollments
    -> WHERE UserID IN (
    ->     SELECT UserID
    ->     FROM Users
    ->     WHERE DateOfBirth > '2003-01-01'
    -> )
    -> GROUP BY Trainer;
+-------------------+---------------+
| Trainer           | CoursesTaught |
+-------------------+---------------+
| Dr. Kevin Mitnick |             1 |
| Dr. Ada Lovelace  |             1 |
| Dr. Elon Musk     |             1 |
| Dr. Grace Hopper  |             1 |
+-------------------+---------------+
4 rows in set (0.00 sec)

mysql> 
mysql> -- 14. Max UserID for a Course by a Specific Trainer
mysql> SELECT MAX(UserID) AS MaxUserID
    -> FROM Enrollments
    -> WHERE Trainer = 'Dr. Kevin Mitnick';
+-----------+
| MaxUserID |
+-----------+
|       101 |
+-----------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 15. Sum EnrollmentIDs for Users Whose First Name Starts with 'J'
mysql> SELECT SUM(EnrollmentID) AS TotalFromJ
    -> FROM Enrollments
    -> WHERE UserID IN (
    ->     SELECT UserID
    ->     FROM Users
    ->     WHERE FirstName LIKE 'J%'
    -> );
+------------+
| TotalFromJ |
+------------+
|         16 |
+------------+
1 row in set (0.00 sec)

mysql> 
mysql> -- 16. List Users with Full Name in Uppercase and Age
mysql> SELECT
    ->     CONCAT(UPPER(FirstName), ' ', UPPER(LastName)) AS FullNameUpper,
    ->     DATEDIFF(NOW(), DateOfBirth) DIV 365 AS Age
    -> FROM Users;
+---------------+------+
| FullNameUpper | Age  |
+---------------+------+
| ALICE WALKER  |   23 |
| JACK SMITH    |   22 |
| JENNY BROWN   |   20 |
| DAVID LEE     |   23 |
| JULIA WHITE   |   24 |
+---------------+------+
5 rows in set (0.00 sec)

   0000 
