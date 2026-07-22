# HCIA-openGauss Lab Guide

## Logging In to a Database

```shell
student@openGauss~$ su - omm

omm@openGauss~$ gsql -d postgres -p 5432 -r

\q
```

`help`       – help  
`\copyright` – copyright information  
`\h`         – view all SQL statements   
`\l`         – дерекқорлар тізімін көру  
`\c school`  – school дерекқорына қосылу  
`\dt`        – кестелерді көрсету  
`\q`         – шығу  

##  Creating, Querying, Modifying, and Deleting Tablespaces

```shell
# Create a Tablespace Directory

student@openGauss~$ sudo mkdir -p /opt/tablespace
student@openGauss~$ sudo chown -R omm:dbgroup /opt/tablespace
student@openGauss~$ sudo chmod 700 /opt/tablespace
```

```shell
# Create a Tablespace
CREATE TABLESPACE tbspace1 LOCATION '/opt/tablespace/tbspace1';
```

```shell
# Method 1: Query a Tablespace

\db

    Name     | Owner |          Location
-------------+-------+-------------------------
 pg_default  | omm   |
 pg_global   | omm   |
 tbspace1    | omm   | /opt/tablespace/tbspace1


# Method 2: Query a Tablespace

SELECT spcname FROM pg_tablespace;

   spcname
-------------
 pg_default
 pg_global
 tbspace1
```

```shell
# Create a User and Grant the user the permissions to access the tbspace1 Tablespace

CREATE USER user1 IDENTIFIED BY 'Huawei@123';
GRANT CREATE ON TABLESPACE tbspace1 TO user1;
```

```shell
# Method 1: Create a Table in the tbspace1 Tablespace

CREATE TABLE student (
    student_id INT,
    student_name VARCHAR(30),
    student_gender VARCHAR(6),
    student_birth DATE
) TABLESPACE tbspace1;
```

```shell
# Method 2: Create a Table in the tablespace1 Tablespace

SET default_tablespace = tbspace1;

CREATE TABLE teacher (
    teacher_id INT,
    teacher_name VARCHAR(30),
    teacher_jobtitle VARCHAR(20),
    teacher_gender VARCHAR(6),
    teacher_age DATE
);
```

```shell
\dt

Schema |  Name   | Type  | Owner |             Storage
--------+---------+-------+-------+----------------------------------
 public | student | table | omm   | {orientation=row,compression=no}
 public | teacher | table | omm   | {orientation=row,compression=no}
```

```shell
# Query the Tablespace usage

SELECT PG_TABLESPACE_SIZE('tbspace1');

 pg_tablespace_size
--------------------
               8192
```

```shell
# Modify the Tablespace

ALTER TABLESPACE tbspace1 RENAME TO tbs1;

/db

    Name    | Owner |          Location
------------+-------+-------------------------
 pg_default | omm   |
 pg_global  | omm   |
 tbs1       | omm   | /opt/tablespace/tbspace1
```

```shell
# Delete a Tablespace

DROP TABLESPACE tbs1;
ERROR:  tablespace "tbs1" is not empty

DROP TABLE student;

DROP TABLE teacher;

DROP TABLESPACE tbs1;

/db
```

## Creating, Viewing, Modifying, and Deleting a Database

```shell
# Create two Tablespaces tbspace1 and tbspace2

CREATE TABLESPACE tbspace1 LOCATION '/opt/tablespace/tbspace1';
CREATE TABLESPACE tbspace2 LOCATION '/opt/tablespace/tbspace2';
```

```shell
# Create the db1 database in the tbspace1 Tablespace

CREATE DATABASE db1 with TABLESPACE = tbspace1;
```

```shell
# Method 1: View a Database

SELECT datname FROM pg_database;

  datname
-----------
 template1
 school
 finance
 template0
 db1
 postgres

# Method 2: View a Database
\l

   Name    | Owner | Encoding |   Collate   |    Ctype    | Access privileges
-----------+-------+----------+-------------+-------------+-------------------
 db1       | omm   | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 postgres  | omm   | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | omm   | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/omm           +
           |       |          |             |             | omm=CTc/omm
 template1 | omm   | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/omm           +
           |       |          |             |             | omm=CTc/omm
```

```shell
# Modify a Database

# Modify the default search path of the Database
ALTER DATABASE db1 SET search_path TO pa_catalog,public;

# Change the Database Tablespace
ALTER DATABASE db1 SET TABLESPACE tbspace2;

# Rename the Database
ALTER DATABASE db1 RENAME TO db2;
```

```shell
# Delete a Database

DROP DATABASE db2;
\l

   Name    | Owner | Encoding |   Collate   |    Ctype    | Access privileges
-----------+-------+----------+-------------+-------------+-------------------
 postgres  | omm   | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | omm   | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/omm           +
           |       |          |             |             | omm=CTc/omm
 template1 | omm   | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/omm           +
           |       |          |             |             | omm=CTc/omm
```

```shell
```

```shell
```

```shell
```

```shell
```

```shell
```

```shell
```

```shell
```
