 QUESTION 1 : EMPLOYEE table — EMPNO,ENAME,SAL,DEPTNO,DNAME,LOC


 
-- STEP 1: Create original EMPLOYEE table (as given in question)
mysql> CREATE TABLE EMPLOYEE (
-> EMPNO INT PRIMARY KEY,
-> ENAME VARCHAR(50),
-> SAL DECIMAL(10,2),
-> DEPTNO INT,
-> DNAME VARCHAR(50),
-> LOC VARCHAR(50)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 2: Insert sample data
mysql> INSERT INTO EMPLOYEE VALUES
-> (101, 'Alice', 50000, 10, 'HR', 'Mumbai'),
-> (102, 'Bob', 60000, 20, 'Finance', 'Delhi'),
-> (103, 'Carol', 55000, 10, 'HR', 'Mumbai'),
-> (104, 'Dave', 70000, 30, 'IT', 'Pune');
Query OK, 4 rows affected (0.01 sec)
-- STEP 3: View original table (as given in question)
mysql> SELECT * FROM EMPLOYEE;
+-------+-------+----------+--------+---------+---------+
| EMPNO | ENAME | SAL      | DEPTNO | DNAME   | LOC     |
+-------+-------+----------+--------+---------+---------+
| 101   | Alice | 50000.00 | 10     | HR      | Mumbai  |
| 102   | Bob   | 60000.00 | 20     | Finance | Delhi   |
| 103   | Carol | 55000.00 | 10     | HR      | Mumbai  |
| 104   | Dave  | 70000.00 | 30     | IT      | Pune    |
+-------+-------+----------+--------+---------+---------+
4 rows in set (0.00 sec)
-- STEP 4: Create 3NF Table 1 — DEPT (removes transitive dependency)
mysql> CREATE TABLE DEPT (
-> DEPTNO INT PRIMARY KEY,
-> DNAME VARCHAR(50),
-> LOC VARCHAR(50)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 5: Create 3NF Table 2 — EMP (only direct dependencies on EMPNO)
mysql> CREATE TABLE EMP (
-> EMPNO INT PRIMARY KEY,
-> ENAME VARCHAR(50),
-> SAL DECIMAL(10,2),
-> DEPTNO INT,
-> FOREIGN KEY (DEPTNO) REFERENCES DEPT(DEPTNO)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 6: Insert into DEPT first (parent table)
mysql> INSERT INTO DEPT VALUES
-> (10, 'HR', 'Mumbai'),
-> (20, 'Finance', 'Delhi'),
-> (30, 'IT', 'Pune');
Query OK, 3 rows affected (0.01 sec)
-- STEP 7: Insert into EMP (child table)
mysql> INSERT INTO EMP VALUES
-> (101, 'Alice', 50000, 10),
-> (102, 'Bob', 60000, 20),
-> (103, 'Carol', 55000, 10),
-> (104, 'Dave', 70000, 30);
Query OK, 4 rows affected (0.01 sec)
-- STEP 8: View 3NF result (ANSWER)
mysql> SELECT * FROM DEPT;
+--------+---------+---------+
| DEPTNO | DNAME   | LOC     |
+--------+---------+---------+
| 10     | HR      | Mumbai  |
| 20     | Finance | Delhi   |
| 30     | IT      | Pune    |
+--------+---------+---------+
3 rows in set (0.00 sec)
mysql> SELECT * FROM EMP;
+-------+-------+----------+--------+
| EMPNO | ENAME | SAL      | DEPTNO |
+-------+-------+----------+--------+
| 101   | Alice | 50000.00 | 10     |
| 102   | Bob   | 60000.00 | 20     |
| 103   | Carol | 55000.00 | 10     |
| 104   | Dave  | 70000.00 | 30     |
+-------+-------+----------+--------+
4 rows in set (0.00 sec) <-- Transitive dependency removed. 3NF achieved!
mysql> -- JOIN to verify full result
mysql> SELECT e.EMPNO, e.ENAME, e.SAL, d.DEPTNO, d.DNAME, d.LOC
-> FROM EMP e JOIN DEPT d ON e.DEPTNO = d.DEPTNO;
+-------+-------+----------+--------+---------+---------+
| EMPNO | ENAME | SAL      | DEPTNO | DNAME   | LOC     |
+-------+-------+----------+--------+---------+---------+
| 101   | Alice | 50000.00 | 10     | HR      | Mumbai  |
| 102   | Bob   | 60000.00 | 20     | Finance | Delhi   |
| 103   | Carol | 55000.00 | 10     | HR      | Mumbai  |
| 104   | Dave  | 70000.00 | 30     | IT      | Pune    |
+-------+-------+----------+--------+---------+---------+
4 rows in set (0.00 sec)



QUESTION 2 : STUDENT table — ROLLNO,NAME,AGE,EXAM,MARKS,GRADE


-- STEP 1: Create original STUDENT table (as given in question)
mysql> CREATE TABLE STUDENT (
-> ROLLNO INT,
-> NAME VARCHAR(50),
-> AGE INT,
-> EXAM VARCHAR(20),
-> MARKS INT,
-> GRADE VARCHAR(5),
-> PRIMARY KEY (ROLLNO, EXAM)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 2: Insert sample data
mysql> INSERT INTO STUDENT VALUES
-> (1, 'Rohit', 21, 'Maths', 85, 'A'),
-> (1, 'Rohit', 21, 'Science', 72, 'B'),
-> (2, 'Priya', 20, 'Maths', 91, 'A+'),
-> (3, 'Karan', 22, 'Science', 60, 'C');
Query OK, 4 rows affected (0.01 sec)
-- STEP 3: View original table (as given in question)
mysql> SELECT * FROM STUDENT;
+--------+-------+-----+---------+-------+-------+
| ROLLNO | NAME  | AGE | EXAM    | MARKS | GRADE |
+--------+-------+-----+---------+-------+-------+
| 1      | Rohit | 21  | Maths   | 85    | A     |
| 1      | Rohit | 21  | Science | 72    | B     |
| 2      | Priya | 20  | Maths   | 91    | A+    |
| 3      | Karan | 22  | Science | 60    | C     |
+--------+-------+-----+---------+-------+-------+
4 rows in set (0.00 sec) 
-- STEP 4: Create 3NF Table 1 — MARKS_GRADE (removes transitive dependency)
mysql> CREATE TABLE MARKS_GRADE (
-> MARKS INT PRIMARY KEY,
-> GRADE VARCHAR(5)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 5: Create 3NF Table 2 — STUDENT_3NF
mysql> CREATE TABLE STUDENT_3NF (
-> ROLLNO INT,
-> NAME VARCHAR(50),
-> AGE INT,
-> EXAM VARCHAR(20),
-> MARKS INT,
-> PRIMARY KEY (ROLLNO, EXAM),
-> FOREIGN KEY (MARKS) REFERENCES MARKS_GRADE(MARKS)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 6: Insert into MARKS_GRADE (parent table)
mysql> INSERT INTO MARKS_GRADE VALUES
-> (85, 'A'),
-> (72, 'B'),
-> (91, 'A+'),
-> (60, 'C');
Query OK, 4 rows affected (0.01 sec)
-- STEP 7: Insert into STUDENT_3NF (child table)
mysql> INSERT INTO STUDENT_3NF VALUES
-> (1, 'Rohit', 21, 'Maths', 85),
-> (1, 'Rohit', 21, 'Science', 72),
-> (2, 'Priya', 20, 'Maths', 91),
-> (3, 'Karan', 22, 'Science', 60);
Query OK, 4 rows affected (0.01 sec)
-- STEP 8: View 3NF result (ANSWER)
mysql> SELECT * FROM MARKS_GRADE;
+-------+-------+
| MARKS | GRADE |
+-------+-------+
| 60    | C     |
| 72    | B     |
| 85    | A     |
| 91    | A+    |
+-------+-------+
4 rows in set (0.00 sec)
mysql> SELECT * FROM STUDENT_3NF;
+--------+-------+-----+---------+-------+
| ROLLNO | NAME  | AGE | EXAM    | MARKS |
+--------+-------+-----+---------+-------+
| 1      | Rohit | 21  | Maths   | 85    |
| 1      | Rohit | 21  | Science | 72    |
| 2      | Priya | 20  | Maths   | 91    |
| 3      | Karan | 22  | Science | 60    |
+--------+-------+-----+---------+-------+
4 rows in set (0.00 sec) 
mysql> -- JOIN to verify full result
mysql> SELECT s.ROLLNO, s.NAME, s.AGE, s.EXAM, s.MARKS, mg.GRADE
-> FROM STUDENT_3NF s JOIN MARKS_GRADE mg ON s.MARKS = mg.MARKS;
+--------+-------+-----+---------+-------+-------+
| ROLLNO | NAME  | AGE | EXAM    | MARKS | GRADE |
+--------+-------+-----+---------+-------+-------+
| 1      | Rohit | 21  | Maths   | 85    | A     |
| 1      | Rohit | 21  | Science | 72    | B     |
| 2      | Priya | 20  | Maths   | 91    | A+    |
| 3      | Karan | 22  | Science | 60    | C     |
+--------+-------+-----+---------+-------+-------+
4 rows in set (0.00 sec)




QUESTION 3 : EMPLOYEE — EMPNO,PROJECT_NO,NO_OF_DAYS,CUSTOMERNAME


-- STEP 1: Create original EMPLOYEE table (as given in question)
mysql> CREATE TABLE EMP_PROJECT (
-> EMPNO INT,
-> PROJECT_NO INT,
-> NO_OF_DAYS INT,
-> CUSTOMERNAME VARCHAR(50),
-> PRIMARY KEY (EMPNO, PROJECT_NO)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 2: Insert sample data
mysql> INSERT INTO EMP_PROJECT VALUES
-> (101, 1, 30, 'Infosys'),
-> (101, 2, 45, 'TCS'),
-> (102, 1, 20, 'Infosys'),
-> (103, 3, 60, 'Wipro');
Query OK, 4 rows affected (0.01 sec)
-- STEP 3: View original table (as given in question)
mysql> SELECT * FROM EMP_PROJECT;
+-------+------------+------------+--------------+
| EMPNO | PROJECT_NO | NO_OF_DAYS | CUSTOMERNAME |
+-------+------------+------------+--------------+
| 101   | 1          | 30         | Infosys      |
| 101   | 2          | 45         | TCS          |
| 102   | 1          | 20         | Infosys      |
| 103   | 3          | 60         | Wipro        |
+-------+------------+------------+--------------+
4 rows in set (0.00 sec)
-- STEP 4: Create 2NF Table 1 — PROJECT (removes partial dependency)
mysql> CREATE TABLE PROJECT (
-> PROJECT_NO INT PRIMARY KEY,
-> CUSTOMERNAME VARCHAR(50)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 5: Create 2NF Table 2 — EMP_PROJECT_2NF
mysql> CREATE TABLE EMP_PROJECT_2NF (
-> EMPNO INT,
-> PROJECT_NO INT,
-> NO_OF_DAYS INT,
-> PRIMARY KEY (EMPNO, PROJECT_NO),
-> FOREIGN KEY (PROJECT_NO) REFERENCES PROJECT(PROJECT_NO)
-> );
Query OK, 0 rows affected (0.02 sec)
-- STEP 6: Insert into PROJECT first (parent table)
mysql> INSERT INTO PROJECT VALUES
-> (1, 'Infosys'),
-> (2, 'TCS'),
-> (3, 'Wipro');
Query OK, 3 rows affected (0.01 sec)
-- STEP 7: Insert into EMP_PROJECT_2NF (child table)
mysql> INSERT INTO EMP_PROJECT_2NF VALUES
-> (101, 1, 30),
-> (101, 2, 45),
-> (102, 1, 20),
-> (103, 3, 60);
Query OK, 4 rows affected (0.01 sec)
-- STEP 8: View 2NF result (ANSWER)
mysql> SELECT * FROM PROJECT;
+------------+--------------+
| PROJECT_NO | CUSTOMERNAME |
+------------+--------------+
| 1          | Infosys      |
| 2          | TCS          |
| 3          | Wipro        |
+------------+--------------+
3 rows in set (0.00 sec)
mysql> SELECT * FROM EMP_PROJECT_2NF;
+-------+------------+------------+
| EMPNO | PROJECT_NO | NO_OF_DAYS |
+-------+------------+------------+
| 101   | 1          | 30         |
| 101   | 2          | 45         |
| 102   | 1          | 20         |
| 103   | 3          | 60         |
+-------+------------+------------+
4 rows in set (0.00 sec)
mysql> -- JOIN to verify full result
mysql> SELECT e.EMPNO, e.PROJECT_NO, e.NO_OF_DAYS, p.CUSTOMERNAME
-> FROM EMP_PROJECT_2NF e JOIN PROJECT p ON e.PROJECT_NO = p.PROJECT_NO;
+-------+------------+------------+--------------+
| EMPNO | PROJECT_NO | NO_OF_DAYS | CUSTOMERNAME |
+-------+------------+------------+--------------+
| 101   | 1          | 30         | Infosys      |
| 101   | 2          | 45         | TCS          |
| 102   | 1          | 20         | Infosys      |
| 103   | 3          | 60         | Wipro        |
+-------+------------+------------+--------------+
4 rows in set (0.00 sec)
mysql> _
