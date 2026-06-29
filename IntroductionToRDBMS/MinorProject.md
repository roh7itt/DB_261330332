-- QUESTION 1 : Normalize into 1st Normal Form (1NF)


-- STEP 1: Create original table (as given in question)
mysql> CREATE TABLE Member (
-> Member_Id INT,
-> First_Name VARCHAR(50),
-> Last_Name VARCHAR(50),
-> Hobbies VARCHAR(100)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 2: Insert data as given in question
mysql> INSERT INTO Member VALUES
-> (101, 'Jayson', 'Mark', 'Cricket,Swimming,Football'),
-> (102, 'Ram', 'Ganesh', 'Swimming,Running,Music'),
-> (103, 'Raj', 'Kishore', 'Dancing,Singing,Running');
Query OK, 3 rows affected (0.01 sec)
-- STEP 3: View original table (same as question)
mysql> SELECT * FROM Member;
+-----------+------------+-----------+---------------------------+
| Member_Id | First_Name | Last_Name | Hobbies                   |
+-----------+------------+-----------+---------------------------+
| 101       | Jayson     | Mark      | Cricket,Swimming,Football |
| 102       | Ram        | Ganesh    | Swimming,Running,Music    |
| 103       | Raj        | Kishore   | Dancing,Singing,Running   |
+-----------+------------+-----------+---------------------------+
3 rows in set (0.00 sec) <-- Hobbies has multiple values. NOT in 1NF!
-- STEP 4: Create normalized 1NF table
mysql> CREATE TABLE Member_1NF (
-> Member_Id INT,
-> First_Name VARCHAR(50),
-> Last_Name VARCHAR(50),
-> Hobby VARCHAR(50),
-> PRIMARY KEY (Member_Id, Hobby)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 5: Insert atomic (split) values into 1NF table
mysql> INSERT INTO Member_1NF VALUES
-> (101, 'Jayson', 'Mark', 'Cricket'),
-> (101, 'Jayson', 'Mark', 'Swimming'),
-> (101, 'Jayson', 'Mark', 'Football'),
-> (102, 'Ram', 'Ganesh', 'Swimming'),
-> (102, 'Ram', 'Ganesh', 'Running'),
-> (102, 'Ram', 'Ganesh', 'Music'),
-> (103, 'Raj', 'Kishore', 'Dancing'),
-> (103, 'Raj', 'Kishore', 'Singing'),
-> (103, 'Raj', 'Kishore', 'Running');
Query OK, 9 rows affected (0.01 sec)
-- STEP 6: View 1NF result (ANSWER)
mysql> SELECT * FROM Member_1NF;
+-----------+------------+-----------+----------+
| Member_Id | First_Name | Last_Name | Hobby    |
+-----------+------------+-----------+----------+
| 101       | Jayson     | Mark      | Cricket  |
| 101       | Jayson     | Mark      | Swimming |
| 101       | Jayson     | Mark      | Football |
| 102       | Ram        | Ganesh    | Swimming |
| 102       | Ram        | Ganesh    | Running  |
| 102       | Ram        | Ganesh    | Music    |
| 103       | Raj        | Kishore   | Dancing  |
| 103       | Raj        | Kishore   | Singing  |
| 103       | Raj        | Kishore   | Running  |
+-----------+------------+-----------+----------+
9 rows in set (0.00 sec) <-- Each cell is atomic. 1NF achieved!




-- QUESTION 2 : Normalize into 2nd Normal Form (2NF)


-- STEP 1: Create original table (as given in question)
mysql> CREATE TABLE Employee (
-> Empno INT,
-> Training VARCHAR(50),
-> Training_Date DATE,
-> Dept VARCHAR(10),
-> PRIMARY KEY (Empno, Training)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 2: Insert data as given in question
mysql> INSERT INTO Employee VALUES
-> (101, 'Oracle SQL', '2015-08-12', 'TT'),
-> (101, 'Java', '2015-08-21', 'BU'),
-> (102, 'Oracle SQL', '2014-09-18', 'TT');
Query OK, 3 rows affected (0.01 sec)
-- STEP 3: View original table (same as question)
mysql> SELECT * FROM Employee;
+-------+------------+---------------+------+
| Empno | Training   | Training_Date | Dept |
+-------+------------+---------------+------+
| 101   | Oracle SQL | 2015-08-12    | TT   |
| 101   | Java       | 2015-08-21    | BU   |
| 102   | Oracle SQL | 2014-09-18    | TT   |
+-------+------------+---------------+------+
3 rows in set (0.00 sec) <-- Dept depends only on Empno. Partial dependency! NOT in 2NF
-- STEP 4: Create 2NF table 1 — Employee_Training
mysql> CREATE TABLE Employee_Training (
-> Empno INT,
-> Training VARCHAR(50),
-> Training_Date DATE,
-> PRIMARY KEY (Empno, Training)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 5: Create 2NF table 2 — Employee_Dept
mysql> CREATE TABLE Employee_Dept (
-> Empno INT PRIMARY KEY,
-> Dept VARCHAR(10)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 6: Insert into Employee_Training
mysql> INSERT INTO Employee_Training VALUES
-> (101, 'Oracle SQL', '2015-08-12'),
-> (101, 'Java', '2015-08-21'),
-> (102, 'Oracle SQL', '2014-09-18');
Query OK, 3 rows affected (0.01 sec)
-- STEP 7: Insert into Employee_Dept
mysql> INSERT INTO Employee_Dept VALUES
-> (101, 'TT'),
-> (101, 'BU'),
-> (102, 'TT');
Query OK, 3 rows affected (0.01 sec)
-- STEP 8: View 2NF result (ANSWER)
mysql> SELECT * FROM Employee_Training;
+-------+------------+---------------+
| Empno | Training   | Training_Date |
+-------+------------+---------------+
| 101   | Oracle SQL | 2015-08-12    |
| 101   | Java       | 2015-08-21    |
| 102   | Oracle SQL | 2014-09-18    |
+-------+------------+---------------+
3 rows in set (0.00 sec)
mysql> SELECT * FROM Employee_Dept;
+-------+------+
| Empno | Dept |
+-------+------+
| 101   | TT   |
| 101   | BU   |
| 102   | TT   |
+-------+------+
3 rows in set (0.00 sec) <-- Partial dependency removed. 2NF achieved!
mysql> -- JOIN to verify full result
mysql> SELECT et.Empno, et.Training, et.Training_Date, ed.Dept
-> FROM Employee_Training et
-> JOIN Employee_Dept ed ON et.Empno = ed.Empno;
+-------+------------+---------------+------+
| Empno | Training   | Training_Date | Dept |
+-------+------------+---------------+------+
| 101   | Oracle SQL | 2015-08-12    | TT   |
| 101   | Java       | 2015-08-21    | BU   |
| 102   | Oracle SQL | 2014-09-18    | TT   |
+-------+------------+---------------+------+
3 rows in set (0.00 sec)



-- QUESTION 3 : Normalize into 3rd Normal Form (3NF)


-- STEP 1: Create original table (as given in question)
mysql> CREATE TABLE Member_Sports (
-> Member_Id INT PRIMARY KEY,
-> First_Name VARCHAR(50),
-> Last_Name VARCHAR(50),
-> Sports VARCHAR(50),
-> Fees INT
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 2: Insert data as given in question
mysql> INSERT INTO Member_Sports VALUES
-> (101, 'Rajesh', 'Chand', 'Cricket', 100),
-> (102, 'Jayesh', 'Raj', 'Hockey', 80),
-> (103, 'Mark', 'Dorson', 'Foodball', 90);
Query OK, 3 rows affected (0.01 sec)
-- STEP 3: View original table (same as question)
mysql> SELECT * FROM Member_Sports;
+-----------+------------+-----------+----------+------+
| Member_Id | First_Name | Last_Name | Sports   | Fees |
+-----------+------------+-----------+----------+------+
| 101       | Rajesh     | Chand     | Cricket  | 100  |
| 102       | Jayesh     | Raj       | Hockey   | 80   |
| 103       | Mark       | Dorson    | Foodball | 90   |
+-----------+------------+-----------+----------+------+
3 rows in set (0.00 sec) <-- Fees depends on Sports not Member_Id. Transitive dependency! NOT in 3NF
-- STEP 4: Create 3NF table 1 — Sports_Fees (PK: Sports)
mysql> CREATE TABLE Sports_Fees (
-> Sports VARCHAR(50) PRIMARY KEY,
-> Fees INT
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 5: Create 3NF table 2 — Member (PK: Member_Id)
mysql> CREATE TABLE Member_3NF (
-> Member_Id INT PRIMARY KEY,
-> First_Name VARCHAR(50),
-> Last_Name VARCHAR(50),
-> Sports VARCHAR(50),
-> FOREIGN KEY (Sports) REFERENCES Sports_Fees(Sports)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 6: Insert into Sports_Fees first (parent table)
mysql> INSERT INTO Sports_Fees VALUES
-> ('Cricket', 100),
-> ('Hockey', 80),
-> ('Foodball', 90);
Query OK, 3 rows affected (0.01 sec)
-- STEP 7: Insert into Member_3NF (child table)
mysql> INSERT INTO Member_3NF VALUES
-> (101, 'Rajesh', 'Chand', 'Cricket'),
-> (102, 'Jayesh', 'Raj', 'Hockey'),
-> (103, 'Mark', 'Dorson', 'Foodball');
Query OK, 3 rows affected (0.01 sec)
-- STEP 8: View 3NF result (ANSWER)
mysql> SELECT * FROM Sports_Fees;
+----------+------+
| Sports   | Fees |
+----------+------+
| Cricket  | 100  |
| Hockey   | 80   |
| Foodball | 90   |
+----------+------+
3 rows in set (0.00 sec)
mysql> SELECT * FROM Member_3NF;
+-----------+------------+-----------+----------+
| Member_Id | First_Name | Last_Name | Sports   |
+-----------+------------+-----------+----------+
| 101       | Rajesh     | Chand     | Cricket  |
| 102       | Jayesh     | Raj       | Hockey   |
| 103       | Mark       | Dorson    | Foodball |
+-----------+------------+-----------+----------+
3 rows in set (0.00 sec) <-- Transitive dependency removed. 3NF achieved!
mysql> -- JOIN to verify full result
mysql> SELECT m.Member_Id, m.First_Name, m.Last_Name, m.Sports, sf.Fees
-> FROM Member_3NF m
-> JOIN Sports_Fees sf ON m.Sports = sf.Sports;
+-----------+------------+-----------+----------+------+
| Member_Id | First_Name | Last_Name | Sports   | Fees |
+-----------+------------+-----------+----------+------+
| 101       | Rajesh     | Chand     | Cricket  | 100  |
| 102       | Jayesh     | Raj       | Hockey   | 80   |
| 103       | Mark       | Dorson    | Foodball | 90   |
+-----------+------------+-----------+----------+------+
3 rows in set (0.00 sec)
mysql> _