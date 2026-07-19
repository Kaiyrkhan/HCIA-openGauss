# Server-based installation on a Single Node

| item                        | value                           |
|-----------------------------|---------------------------------|
| Operating System            | openEuler 22.03 LTS SP4 x86_64  |
| openGauss Database          | openGauss Server 6.0.5 x86_64   |
| Hostname                    | openGauss                       |
| Linux User                  | omm                             |
| Linux Group                 | dbgroup                         |
| openGauss Port              | 5432/TCP                        |
| installation Path           | /opt/openGauss                  |
| Data Directory              | /opt/openGauss/data/single_node |
| openGauss Database User     | omm                             |
| openGauss Database Password | Huawei@123                      |


**Hostname**
```shell
student@openGauss~$ sudo vi /etc/hosts
Shift+} -> Edit mode -> Enter
192.168.0.103 openGauss
Normal mode -> :wq
```

```shell
student@openGauss~$ ping -c2 openGauss
64 bytes from openGauss (192.168.0.103): icmp_seq=1 ttl=64 time=0.035 ms
64 bytes from openGauss (192.168.0.103): icmp_seq=2 ttl=64 time=0.101 ms
```

**Configure openGauss User and Group**
```shell
student@openGauss~$ sudo groupadd dbgroup

student@openGauss~$ sudo useradd -g dbgroup omm

student@openGauss~$ sudo passwd omm
New Password: 123
```

```shell
student@openGauss~$ id omm
student@openGauss~$ getent passwd | grep omm
student@openGauss~$ getent group | grep dbgroup
```
  
```shell
student@openGauss~$ ss -tulpn | grep 5432

student@openGauss~$ netstat -tulpn | grep 5432
```

**DeCompress**
```shell
student@openGauss~$ ls -lh
openGauss-Server-6.0.5-openEuler22.03-x86_64.tar.bz2
```

```shell
student@openGauss~$ sudo mkdir /opt/openGauss

student@openGauss~$ sudo tar -xf openGauss-Server-6.0.5-openEuler22.03-x86_64.tar.bz2 -C /opt/openGauss

student@openGauss~$ ls -l /opt/openGauss/
drwxr-xr-x  root root   bin
drwxr-xr-x  root root   etc
drwxr-xr-x  root root   include
drwxr-xr-x  root root   lib
drwxr-xr-x  root root   share
drwxr-xr-x  root root   simpleInstall
-rw-r--r--  root root   version.cfg
```

**Permission**
```shell
student@openGauss~$ sudo chown -R omm:dbgroup /opt/openGauss
student@openGauss~$ sudo chmod -R 755 /opt/openGauss

student@openGauss~$ ls -l /opt
drwxr-xr-x  omm dbgroup openGauss
```

**install openGauss**
```shell
student@openGauss~$ su - omm
password: 123
omm@openGauss~$

omm@openGauss~$ cd /opt/openGauss/simpleInstall

omm@openGauss~$ sh install.sh -w "openGauss@123"
Would you like to create a demo database (yes/no)? yes
```
`-w` – мәліметтер қорына құпиясөз орнату  

```shell
omm@openGauss~$ source ~/.bashrc
-bash: ulimit: open files: cannot modify limit: Operation not permitted

omm@openGauss~$ vi ~/.bashrc
# ulimit -n 1000000
:wq

omm@openGauss~$ source ~/.bashrc
```
`source ~/.bashrc` – жаңадан қосылған айнымалыларды (мысалы: GAUSSHOME, gsql) жүйеге енгізеді  

**Verify Database**

> Database Directory: **/opt/openGauss/data/single_node**  

```shell
omm@openGauss~$ ps ux | grep gaussdb
/opt/openGauss/bin/gaussdb -D /opt/openGauss/data/single_node
```
![images](./images/opengauss_ps.png)

```shell
omm@openGauss~$ gs_ctl query -D /opt/openGauss/data/single_node
Normal
```
![images](./images/opengauss_gsctl-query.png)

```shell
omm@openGauss~$ nano .bashrc
echo -e "Run the following command to log in to the postgres database:"
echo -e "  \033[1;33mgsql -d postgres -p 5432 -r\033[0m\n"

echo -e "openGauss database user: \033[1;33momm\033[0m"
echo -e "openGauss database password: \033[1;33mHuawei@123\033[0m\n"

student@openGauss~$ sudo nano -l /etc/profile.d/system-info.sh
84 else
85     echo -e "\n"
```

**Connecting to openGauss**

```shell
omm@openGauss~$ gsql -d postgres -p 5432 -r

SELECT version();
\q
```
`\copyright` – openGauss version  
`h` – help    
`\l` – дерекқорлардың тізімін көру  
`\c school` – school дерекқорына қосылу  
`\c finance` – finance дерекқорына қосылу  
`\dt` – кестелерді көрсету  
`\q` – шығу  

> Local Connection  
> gsql -d postgres -r  
> gsql -d postgres -p 5432 -r  
> gsql -d postgres -U omm -p 5432 -r  
> gsql -d postgres -U omm -W "openGauss@123" -p 5432 -r  

> Remote Connection  
> gsql -h 192.168.0.103 -d postgres -U omm -p 5432 -r  

**Қосымша ақпарат!**

```shell
# Құпиясөзді өзгерту

ALTER ROLE omm IDENTIFIED BY 'new_password' REPLACE 'old_password';
```

```shell
# Error SEMMNI

student@openGauss~$ sysctl -w kernel.sem="250 85000 250 330"
немесе
student@openGauss~$ sudo vi /etc/sysctl.conf
kernel.sem="250 85000 250 330"
:wq
```

```shell
# import Demo Database

omm@openGauss~$ cd /opt/openGauss/simpleInstall
omm@openGauss~$ gsql -d postgres -U omm -W "openGauss@123" -f school.sql
omm@openGauss~$ gsql -d postgres -U omm -W "openGauss@123" -f finance.sql
```

```shell
# Configure Firewalld

student@openGauss~$ sudo systemctl enable --now firewalld
student@openGauss~$ sudo firewall-cmd --permanent --add-port=5432/tcp
student@openGauss~$ sudo firewall-cmd --reload

$ ss -tulpn | grep 5432
$ netstat -tulpn | grep 5432
```

```shell
omm@openGauss~$ echo $GAUSSHOME
/opt/openGauss
```

```shell
# openGauss Database
# status | start | stop | restart | reload

Status
gs_ctl query -D $GAUSSHOME/data/single_node

Stop
gs_ctl stop -D $GAUSSHOME/data/single_node -m fast

Start
gs_ctl start -D $GAUSSHOME/data/single_node -Z single_node

Restart
gs_ctl restart -D $GAUSSHOME/data/single_node -Z single_node

Reload
gs_ctl reload -D $GAUSSHOME/data/single_node
```

```shell
# pg_hba.conf файлдың ішіндегі "trust" мәнін "sha256" мәніне өзгертетін болсаңыз, database-ге кірген кезде құпиясөзді сұрайтын болады

omm@openGauss~$ grep "trust" /opt/openGauss/data/single_node/pg_hba.conf

TYPE    DATABASE    USER    ADDRESS        METHOD
local   all         all                    trust
host    all         all     127.0.0.1/32   trust
host    all         all     ::1/128        trust

omm@openGauss~$ nano /opt/openGauss/data/single_node/pg_hba.conf

TYPE    DATABASE    USER    ADDRESS        METHOD
local   all         all                    sha256
host    all         all     127.0.0.1/32   sha256
host    all         all     ::1/128        sha256

omm@openGauss~$ gs_ctl reload -D $GAUSSHOME/data/single_node
omm@openGauss~$ gs_ctl query -D $GAUSSHOME/data/single_node

# Нәтижені тексеру
omm@openGauss~$ gsql -d postgres -p 5432 -r
omm@openGauss~$ gsql -d school -p 5432 -r
omm@openGauss~$ gsql -d finance -p 5432 -r
```

```shell
# "NOTICE: The password has been expired, please change the password" - мұндай хабарлама шықпау үшін, төмендегі конфигурацияны жасау керек!

omm@openGauss~$ gs_guc reload -D $GAUSSHOME/data/single_node -c "password_effect_time = 0"
omm@openGauss~$ gs_guc reload -D $GAUSSHOME/data/single_node -c "password_notify_time = 0"

# Нәтижені тексеру
omm@openGauss~$ gsql -d postgres -p 5432 -r -c "SHOW password_effect_time;"
omm@openGauss~$ gsql -d postgres -p 5432 -r -c "SHOW password_notify_time;"
omm@openGauss~$ grep -n "password_effect_time" $GAUSSHOME/data/single_node/postgresql.conf
omm@openGauss~$ grep -n "password_notify_time" $GAUSSHOME/data/single_node/postgresql.conf
```


```shell
# "omm" қолданушы жүйені "Reboot" немесе "Shutdown" жасай алу үшін, төмендегі конфигурацияны жасау керек!

student@openGauss~$ sudo vim /etc/sudoers
немесе
student@openGauss~$ sudo visudo
omm ALL=(ALL) NOPASSWD: /sbin/reboot, /sbin/shutdown, /sbin/poweroff
:wq

omm@openGauss~$ sudo reboot
omm@openGauss~$ sudo shutdown -h now
omm@openGauss~$ sudo poweroff
```

```shell
# Create systemd Service

student@openGauss~$ sudo vi /etc/systemd/system/opengauss.service

[Unit]
Description=openGauss Single Node Database
After=network.target

[Service]
Type=forking
User=omm
Group=dbgroup
ExecStart=/bin/bash -lc 'source /home/omm/.bashrc; gs_ctl start -D /opt/openGauss/data/single_node -Z single_node'
ExecStop=/bin/bash -lc 'source /home/omm/.bashrc; gs_ctl stop -D /opt/openGauss/data/single_node -m fast'
ExecReload=/bin/bash -lc 'source /home/omm/.bashrc; gs_ctl restart -D /openGauss/data/single_node -Z single_node'
TimeoutSec=300

[Install]
WantedBy=multi-user.target

:wq

student@openGauss~$ sudo systemctl daemon-reload
student@openGauss~$ sudo systemctl enable opengauss

student@openGauss~$ su - omm -c 'source ~/.bashrc; gs_ctl stop -D /opt/openGauss/data/single_node -m fast'
server stopped

student@openGauss~$ sudo systemctl start opengauss
student@openGauss~$ sudo systemctl status opengauss --no-pager
```

**Clear Bash History**
```shell
student@openGauss~$ history

student@openGauss~$ ls -la
student@openGauss~$ cat /dev/null > ~/.bash_history
student@openGauss~$ history -c
```

**VMware Workstation -> Description**
```shell
Username: omm
Password: 123

Username: student
Password: 123

Username: root
Password: P@s$w0rd
```

**Take Snapshot**
```shell
Snapshot Manager -> Take Snapshot -> Name: initial image
```

## Logging In to a Database

```shell
student@openGauss~$ su - omm

omm@openGauss~$ gsql -d postgres -p 5432 -r

openGauss=# SELECT version();
openGauss=# \q
```

`\copyright` – openGauss version  
`h` – help    
`\l` – дерекқорлардың тізімін көру  
`\c school` – school дерекқорына қосылу  
`\c finance` – finance дерекқорына қосылу  
`\dt` – кестелерді көрсету  
`\q` – шығу  

##  Creating, Querying, Modifying, and Deleting Tablespaces

```shell
student@openGauss~$ sudo mkdir -p /opt/tablespace/tablespace1
student@openGauss~$ sudo chown -R omm:dbgroup /opt/tablespace
student@openGauss~$ sudo chmod 700 /opt/tablespace/tablespace1
```



```shell
# Create a Tablespace
openGauss=# CREATE TABLESPACE tablespace1 LOCATION '/opt/tablespace/tablespace1';
```

```shell
# Method 1: Query a Tablespace
openGauss=# \db

    Name     | Owner |          Location
-------------+-------+-----------------------------
 pg_default  | omm   |
 pg_global   | omm   |
 tablespace1 | omm   | /opt/tablespace/tablespace1


# Method 2: Query a Tablespace
SELECT spcname FROM pg_tablespace;

   spcname
-------------
 pg_default
 pg_global
 tablespace1
```

```shell
# Create a User and Grant the user the permissions to access the tablespace1 Tablespace

openGauss=# CREATE USER user1 IDENTIFIED BY 'Huawei@123';
openGauss=# GRANT CREATE ON TABLESPACE tablespace1 TO user1;
```

```shell
# Method 1: Create a Table in the tablespace1 Tablespace

CREATE TABLE student (
    student_id INT,
    student_name VARCHAR(30),
    student_birth DATE,
    student_gender VARCHAR(6)
) TABLESPACE tablespace1;
```

```shell
# Method 2: Create a Table in the tablespace1 Tablespace

SET default_tablespace = tablespace1;

CREATE TABLE teacher (
    teacher_id INT,
    teacher_name VARCHAR(30),
    teacher_birth DATE,
    teacher_gender VARCHAR(6)
);
```

```shell
# Query the Tablespace usage

openGauss=# SELECT PG_TABLESPACE_SIZE('tablespace1');

 pg_tablespace_size
--------------------
               8192
```

```shell
# Modify the Tablespace

 openGauss=# ALTER TABLESPACE tablespace1 RENAME TO tbspace1;

 openGauss=# /db

    Name    | Owner |          Location
------------+-------+-----------------------------
 pg_default | omm   |
 pg_global  | omm   |
 tbspace1   | omm   | /opt/tablespace/tablespace1
```

```shell
# Delete a Tablespace

openGauss=# DROP TABLESPACE tbspace1;
ERROR:  tablespace "tbspace1" is not empty

openGauss=# DROP TABLE student;

openGauss=# DROP TABLE teacher;

openGauss=# DROP TABLESPACE tbspace1;
```

```shell
```

```shell
```

```shell
```
