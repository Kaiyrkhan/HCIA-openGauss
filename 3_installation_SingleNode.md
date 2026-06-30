# Server-based installation on a Single Node

WinSCP Download Link: https://winscp.net/eng/download.php  
![images](./images/img.png)  

Create Directory
```shell
$ sudo mkdir /opt/openGauss
$ sudo chown -R omm:dbgroup /opt/openGauss
$ sudo chmod -R 755 /opt/openGauss

$ ls -l /opt
drwxr-xr-x  omm dbgroup openGauss
```

Decompress
```shell
$ sudo dnf install tar
$ sudo tar -xf openGauss-Server-6.0.5-openEuler22.03-x86_64.tar.bz2 -C /opt/openGauss

$ ls -l /opt/openGauss/
drwxr-xr-x  root root   bin
drwxr-xr-x  root root   etc
drwxr-xr-x  root root   include
drwxr-xr-x  root root   lib
drwxr-xr-x  root root   share
drwxr-xr-x  root root   simpleInstall
-rw-r--r--  root root   version.cfg
```

```shell
```

```shell
```

```shell
```
