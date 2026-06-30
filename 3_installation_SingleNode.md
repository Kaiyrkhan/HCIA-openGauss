# Server-based installation on a Single Node

| item                  | value                           |
|-----------------------|---------------------------------|
| Operating System      | openEuler 22.03 LTS SP4 x86_64  |
| Hostname              | openGauss                       |
| Linux Database User   | omm                             |
| Linux Database Group  | dbgroup                         |
| openGauss Port Number | 5432/TCP                        |
| install Path          | /opt/openGauss                  |
| Data Path             | /opt/openGauss/data/single_node |


**Hostname**
```shell
student@openGauss~$ sudo vi /etc/hosts
Shift+} -> Edit mode -> Enter
192.168.0.103 openGauss
Normal mode -> :wq
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

WinSCP Download Link: https://winscp.net/eng/download.php  

![images](./images/winscp_sftp.png)  
![images](./images/winscp_upload.png)  

**DeCompress**
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

omm@openGauss~$ sh install.sh -w "Huawei@123"
Would you like to create a demo database (yes/no)? yes
```
`-w` – мәліметтер қорына құпиясөз орнату  

```shell
omm@openGauss~$ source ~/.bashrc
-bash: ulimit: open files: cannot modify limit: Operation not permitted

omm@openGauss~$ vi .bashrc
# ulimit -n 1000000
:wq

omm@openGauss~$ source ~/.bashrc
```
`source ~/.bashrc` – жаңадан қосылған айнымалыларды (мысалы: GAUSSHOME, gsql) жүйеге енгізеді  

**Verify Database**

> Database Name: **sgnode**  
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
![images](./images/opengauss_gs_ctl.png)

**Connecting to openGauss**

```shell
omm@openGauss~$ gsql -d sgnode -U omm -W "Huawei@123" -p 5432
```
`\l` – дерекқорлардың тізімін көру  
`\c school` – school дерекқорына қосылу  
`\c finance` – finance дерекқорына қосылу  
`\dt` – кестелерді көрсету  
  
  
**Қосымша ақпарат!**

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
omm@openGauss~$ gsql -d sgnode -U omm -W "Huawei@123" -f school.sql
omm@openGauss~$ gsql -d sgnode -U omm -W "Huawei@123" -f finance.sql
```
