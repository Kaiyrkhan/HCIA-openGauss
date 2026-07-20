# Database School

```shell
omm@openGauss~$ gsql -d postgres -p 5432 -r

# Create a Database User
CREATE USER user1 IDENTIFIED BY 'Huawei@123';

# Create a Database
CREATE DATABASE school OWNER user1;

\l

\c school
GRANT USAGE, CREATE ON SCHEMA public TO user1;

\q

omm@openGauss~$ gsql -d school -U user1 -p 5432 -r

CREATE TABLE teacher (
    teacher_id INT,
    teacher_name VARCHAR(30),
    teacher_jobtitle VARCHAR(20),
    teacher_gender VARCHAR(6)
    teacher_age DATE,
);

CREATE TABLE student (
    student_id INT,
    student_name VARCHAR(30),
    student_birth DATE,
    student_gender VARCHAR(6)
);

\dt

 Schema |  Name   | Type  | Owner |             Storage
--------+---------+-------+-------+----------------------------------
 public | student | table | user1 | {orientation=row,compression=no}

INSERT INTO student (student_id, student_name, student_birth, student_gender) VALUES
(1001, 'John', '2012-01-15', 'Male'),
(1002, 'Mary', '2013-03-22', 'Female'),
(1003, 'Michael', '2014-12-30', 'Male'),
(1004, 'Linda', '2010-07-25', 'Female'),
(1005, 'David', '2009-06-10', 'Male'),
(1006, 'Susan', '2014-11-12', 'Female'),
(1007, 'James', '2012-04-04', 'Male'),
(1008, 'Patricia', '2011-09-08', 'Female'),
(1009, 'Robert', '2014-02-11', 'Male'),
(1010, 'Jennifer', '2008-10-01', 'Female');

SELECT * FROM student;

 student_id | student_name |     student_birth     | student_gender
------------+--------------+---------------------+----------------
       1001 | John         | 2012-01-15 00:00:00 | Male
       1002 | Mary         | 2013-03-22 00:00:00 | Female
       1003 | Michael      | 2014-12-30 00:00:00 | Male
       1004 | Linda        | 2010-07-25 00:00:00 | Female
       1005 | David        | 2009-06-10 00:00:00 | Male
       1006 | Susan        | 2014-11-12 00:00:00 | Female
       1007 | James        | 2012-04-04 00:00:00 | Male
       1008 | Patricia     | 2011-09-08 00:00:00 | Female
       1009 | Robert       | 2014-02-11 00:00:00 | Male
       1010 | Jennifer     | 2008-10-01 00:00:00 | Female

\q
```

> GRANT USAGE, CREATE ON SCHEMA public TO user1;  
> GRANT CREATE, USAGE, ALTER, DROP, COMMENT ON SCHEMA public TO user1;  
> GRANT ALL PRIVILEGES ON SCHEMA public TO user1;  
