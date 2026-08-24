mysql> create database if not exists university_db; query ok, 1 row affected (0.01 sec) 
Query OK, 1 row affected (0.01 sec)

    -> use university_db;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'query ok, 1 row affected (0.01 sec) 
use university_db' at line 1
mysql> use university_db;
Database changed
mysql> create table students (student_id int auto_increment primary key, name varchar(100) not null, email varchar(100) not null unique);
Query OK, 0 rows affected (0.05 sec)

mysql> create table course(course_id int auto_increment primary key, course_namr varchar(100) not null, credits int not null, check (credits> 0 and credits <= 6));
Query OK, 0 rows affected (0.05 sec)

mysql> create table enrollments(enrollment_id int auto_increment primary key, student_id int not null, course_id int not null, enrollment_date date not null default (current_date), foregin key (student_id) references students(student_id) on delete cascade on update cascade, foreign key (course_id) references courses (course_id) on delete cascade on update cascade, unique (student_id, course_id));
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'key (student_id) references students(student_id) on delete cascade on update cas' at line 1
mysql> create table enrollments(enrollment_id int auto_increment primary key, student_id int not null, course_id int not null, enrollment_date date not null default (current_date), foregin key (student_id) references students(student_id) on delete cascade on update cascade, foreign key (course_id) references courses (course_id) on delete cascade on update cascade, unique (student_id, course_id));
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'key (student_id) references students(student_id) on delete cascade on update cas' at line 1
mysql> create table enrollments(enrollment_id int auto_increment primary key, student_id int not null, course_id int not null, enrollment_date date not null default (current_date), foreign key (student_id) references students(student_id) on delete cascade on update cascade, foreign key (course_id) references courses (course_id) on delete cascade on update cascade, unique (student_id, course_id));
ERROR 1824 (HY000): Failed to open the referenced table 'courses'
mysql> create table enrollments(enrollment_id int auto_increment primary key, student_id int not null, course_id int not null, enrollment_date date not null default (current_date), foreign key (student_id) references students(student_id) on delete cascade on update cascade, foreign key (course_id) references courses (course_id) on delete cascade on update cascade, unique (student_id, course_id));
ERROR 1824 (HY000): Failed to open the referenced table 'courses'
mysql> create table enrollments(enrollment_id int auto_increment primary key, student_id int not null, course_id int not null, enrollment_date date not null default (current_date), foreign key (student_id) references students(student_id) on delete cascade on update cascade, foreign key (course_id) references courses (course_id) on delete cascade on update cascade, unique (student_id, course_id));
ERROR 1824 (HY000): Failed to open the referenced table 'courses'
mysql> reference table;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'reference table' at line 1
mysql> create table enrollments(enrollment_id int auto_increment primary key, student_id int not null, course_id int not null, enrollment_date date not null default (current_date), foreign key (student_id) references students(student_id) on delete cascade on update cascade, foreign key (course_id) references courses (course_id) on delete cascade on update cascade, unique (student_id, course_id));
ERROR 1824 (HY000): Failed to open the referenced table 'courses'
mysql> show tables;
+-------------------------+
| Tables_in_university_db |
+-------------------------+
| course                  |
| students                |
+-------------------------+
2 rows in set (0.00 sec)

mysql> references courses(course_id)
    -> 
    -> ^C
mysql> references courses(course_id)
    -> ^C
mysql> create table enrollments(enrollment_id int auto_increment primary key, student_id int not null, course_id int not null, enrollment_date date not null default (current_date), foreign key (student_id) references students(student_id) on delete cascade on update cascade, foreign key (course_id) references course (course_id) on delete cascade on update cascade, unique (student_id, course_id));
Query OK, 0 rows affected (0.06 sec)

mysql> insert into students (name, email)values ('alice joshup', 'alice@example.com'), ('bob smith', 'bob@example.com');
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select*from students;
+------------+--------------+-------------------+
| student_id | name         | email             |
+------------+--------------+-------------------+
|          1 | alice joshup | alice@example.com |
|          2 | bob smith    | bob@example.com   |
+------------+--------------+-------------------+
2 rows in set (0.00 sec)

mysql> insert into courses(course_name, credits)values ('database system',3), ('computer network',4);
ERROR 1146 (42S02): Table 'university_db.courses' doesn't exist
mysql> insert into course(course_name, credits)values ('database system',3), ('computer network',4);
ERROR 1054 (42S22): Unknown column 'course_name' in 'field list'
mysql> insert into course(course_name, credits)values ('database system',3), ('computer networks',4);
ERROR 1054 (42S22): Unknown column 'course_name' in 'field list'
mysql> insert into course(course_namr, credits)values ('database system',3), ('computer networks',4);
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from courses;
ERROR 1146 (42S02): Table 'university_db.courses' doesn't exist
mysql> select * from course;
+-----------+-------------------+---------+
| course_id | course_namr       | credits |
+-----------+-------------------+---------+
|         1 | database system   |       3 |
|         2 | computer networks |       4 |
+-----------+-------------------+---------+
2 rows in set (0.00 sec)

mysql> insert into enrollments(student_id, course_id)values(1,10,alice in database systems (2,1), bob in database systems (1,2);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'database systems (2,1), bob in database systems (1,2)' at line 1
mysql> insert into enrollments(student_id, course_id)values(1,10), (2,1), (1,2);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`university_db`.`enrollments`, CONSTRAINT `enrollments_ibfk_2` FOREIGN KEY (`course_id`) REFERENCES `course` (`course_id`) ON DELETE CASCADE ON UPDATE CASCADE)
mysql> insert into enrollments(student_id, course_id)values(1,1), (2,1), (1,2);
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> select*from enrollments;
+---------------+------------+-----------+-----------------+
| enrollment_id | student_id | course_id | enrollment_date |
+---------------+------------+-----------+-----------------+
|             4 |          1 |         1 | 2026-07-28      |
|             5 |          2 |         1 | 2026-07-28      |
|             6 |          1 |         2 | 2026-07-28      |
+---------------+------------+-----------+-----------------+
3 rows in set (0.00 sec)

mysql> delete from courses where course_id=2;
ERROR 1146 (42S02): Table 'university_db.courses' doesn't exist
mysql> delete from course where course_id=2;
Query OK, 1 row affected (0.01 sec)

mysql> select * from students;
+------------+--------------+-------------------+
| student_id | name         | email             |
+------------+--------------+-------------------+
|          1 | alice joshup | alice@example.com |
|          2 | bob smith    | bob@example.com   |
+------------+--------------+-------------------+
2 rows in set (0.00 sec)

mysql> select*from course;
+-----------+-----------------+---------+
| course_id | course_namr     | credits |
+-----------+-----------------+---------+
|         1 | database system |       3 |
+-----------+-----------------+---------+
1 row in set (0.00 sec)

mysql> drop table if exists students;
ERROR 3730 (HY000): Cannot drop table 'students' referenced by a foreign key constraint 'enrollments_ibfk_1' on table 'enrollments'.
mysql> drop table if exixts students;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'exixts students' at line 1
mysql> drop table if exixts enrollments;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'exixts enrollments' at line 1
mysql> drop table if exists enrollments;
Query OK, 0 rows affected (0.03 sec)

mysql> drop table if exists students;
Query OK, 0 rows affected (0.03 sec)

mysql> drop table if exists course;
Query OK, 0 rows affected (0.02 sec)

mysql> drop database university_db;
Query OK, 0 rows affected (0.01 sec)

mysql> 
