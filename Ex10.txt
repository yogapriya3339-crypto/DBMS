mysql> -- create database and table
mysql> CREATE DATABASE IF NOT EXISTS sqli_demo;
Query OK, 1 row affected (0.05 sec) 

mysql> USE sqli_demo;
Database changed 

 mysql> CREATE TABLE IF NOT EXISTS users (    
->   id INT AUTO_INCREMENT PRIMARY KEY, 
->   username VARCHAR(50) NOT NULL,    
->   password VARCHAR(100) NOT NULL,   -- pretend hashed in real apps    
->   role VARCHAR(20) DEFAULT 'user'    
-> );
Query OK, 0 rows affected (0.06 sec) 

mysql> -- sample rows
mysql> INSERT INTO users (username, password, role) VALUES    
->   ('admin','p@ssw0rd','admin'),   
->   ('alice','alicepwd','user'),
->   ('bob','bobpwd','user'); 
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0 

mysql> -- Simulate an application building a query by concatenation: 
mysql> SET @user_input := "admin";    -- normal input
Query OK, 0 rows affected (0.03 sec) 
mysql> SET @vuln_sql := CONCAT("SELECT id,username,passwordFROM users WHERE username = '", @user_input, "';"); 
Query OK, 0 rows affected (0.00 sec)  
mysql> PREPARE p FROM @vuln_sql; 
Query OK, 0 rows affected (0.00 sec)
Statement prepared  

mysql> EXECUTE p;
+----+----------+----------+
| id | username | password |
+----+----------+----------+ 
|  1 | admin    | p@ssw0rd | 
+----+----------+----------+
1 row in set (0.00 sec)

mysql> DEALLOCATE PREPARE p;
Query OK, 0 rows affected (0.00 sec)  

mysql> -- Now simulate malicious input:
mysql> SET @user_input := "' OR '1'='1"; 
Query OK, 0 rows affected (0.00 sec)
mysql> SET @vuln_sql := CONCAT("SELECT id,username,password FROM users WHERE username = '", @user_input, "';");
Query OK, 0 rows affected (0.00 sec)
mysql> PREPARE p2 FROM @vuln_sql; 
Query OK, 0 rows affected (0.00 sec)
Statement prepared 

mysql> EXECUTE p2;
+----+----------+----------+
| id | username | password | 
+----+----------+----------+ 
|  1 | admin    | p@ssw0rd |
|  2 | alice    | alicepwd | 
|  3 | bob      | bobpwd   |
+----+----------+----------+ 
3 rows in set (0.00 sec)  

mysql> DEALLOCATE PREPARE p2;
Query OK, 0 rows affected (0.00 sec) 

mysql> SELECT id,username,password FROM users WHERE username = '' OR '1'='1';
+----+----------+----------+
| id | username | password |
+----+----------+----------+ 
|  1 | admin    | p@ssw0rd | 
|  2 | alice    | alicepwd |
|  3 | bob      | bobpwd   |
+----+----------+----------+
3 rows in set (0.00 sec) 

mysql> -- Use a prepared statement with a placeholder and EXECUTE ... USING 
mysql> SET @sql := 'SELECT id, username FROM users WHERE username = ?'; 
Query OK, 0 rows affected (0.00 sec)
mysql> PREPARE safe_stmt FROM @sql;
Query OK, 0 rows affected (0.03 sec) 
Statement prepared  

mysql> -- safe with normal input
mysql> SET @u := 'admin';
Query OK, 0 rows affected (0.00 sec) 

mysql> EXECUTE safe_stmt USING @u;
+----+----------+
| id | username |
+----+----------+ 
|  1 | admin    |
+----+----------+
1 row in set (0.00 sec) 

mysql> -- try the same attack string, but as a parameter (it will be treated as data)
mysql> SET @u := "' OR '1'='1";
Query OK, 0 rows affected (0.00 sec)
mysql> EXECUTE safe_stmt USING @u;
Empty set (0.00 sec) 
mysql> DEALLOCATE PREPARE safe_stmt;
Query OK, 0 rows affected (0.00 sec)

mysql> DELIMITER $$ 

mysql> CREATE PROCEDURE get_user(IN p_username VARCHAR(50))   
-> BEGIN   
-> -- dynamic SQL using parameter binding    
->   SET @sql := 'SELECT id, username FROM users WHERE username = ?';  
->   PREPARE s FROM @sql;    
->   SET @p := p_username; 
->   EXECUTE s USING @p;    
->   DEALLOCATE PREPARE s;   
-> END$$
Query OK, 0 rows affected (0.04 sec) 

mysql> DELIMITER ;

mysql> -- call normally 
mysql> CALL get_user('alice');
+----+----------+ 
| id | username | 
+----+----------+
|  2 | alice    |
+----+----------+ 
1 row in set (0.00 sec) 
Query OK, 0 rows affected (0.00 sec)

mysql> -- call with attempted injection 
mysql> CALL get_user("' OR '1'='1");
Empty set (0.00 sec)
Query OK, 0 rows affected (0.00 sec) 

mysql> -- Create a view exposing only non-sensitive columns
mysql> CREATE OR REPLACE VIEW v_users AS SELECT id, username, role FROM users; 
Query OK, 0 rows affected (0.01 sec) 

mysql> -- Create a restricted user for the application
mysql> CREATE USER IF NOT EXISTS 'app_user'@'localhost' IDENTIFIED BY 'app_pass';
Query OK, 0 rows affected (0.02 sec) 

mysql> -- Grant the app only SELECT on the view (not on the base table)
mysql> GRANT SELECT ON sqli_demo.v_users TO 'app_user'@'localhost';
Query OK, 0 rows affected (0.01 sec)

mysql> -- (Optional) explicitly revoke direct access to base table 
mysql> REVOKE SELECT ON sqli_demo.users FROM 'app_user'@'localhost'; 
ERROR 1147 (42000): There is no such grant defined for user 'app_user' on host 'localhost' on table 'users'
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec) 

mysql> -- Example: only allow numeric id lookups by checking REGEXP in a stored procedure

mysql> DELIMITER $$ 

mysql> CREATE PROCEDURE get_user_by_id_strict(IN p_id_str VARCHAR(20)) 
-> BEGIN 
->   IF p_id_str REGEXP '^[0-9]+$' THEN 
->   SET @idnum := CAST(p_id_str AS UNSIGNED);  
->   PREPARE q FROM 'SELECT id, username FROM users WHERE id = ?';  
->   EXECUTE q USING @idnum;  
->   DEALLOCATE PREPARE q;  
->   ELSE  
->   SELECT 'ERROR: invalid id format' AS error;   
->   END IF;    
-> END$$
Query OK, 0 rows affected (0.04 sec) 

mysql> DELIMITER ; 

mysql> -- Valid call 
mysql> CALL get_user_by_id_strict('1');
+----+----------+ 
| id | username |
+----+----------+ 
|  1 | admin    |
+----+----------+
1 row in set (0.01 sec) 
Query OK, 0 rows affected (0.01 sec) 

mysql> -- Attack attempt 
mysql> CALL get_user_by_id_strict('1 OR 1=1');
+--------------------------+
| error                    | 
+--------------------------+ 
| ERROR: invalid id format | 
+--------------------------+ 
1 row in set (0.00 sec)  
Query OK, 0 rows affected (0.00 sec) 

mysql> DROP PROCEDURE IF EXISTS get_user;
Query OK, 0 rows affected (0.01 sec) 

mysql> DROP PROCEDURE IF EXISTS get_user_by_id_strict;
Query OK, 0 rows affected (0.01 sec)

mysql> DROP VIEW IF EXISTS v_users;
Query OK, 0 rows affected (0.01 sec) 

mysql> DROP USER IF EXISTS 'app_user'@'localhost';
Query OK, 0 rows affected (0.00 sec) 

mysql> DROP DATABASE IF EXISTS sqli_demo;
Query OK, 1 row affected (0.02 sec) 
