# Preparing the Installation Environment

### Step1 - Obtaining installation Package

**Download Link:** https://opengauss.org/en/download/  

```shell
student@openGauss~$ ls -lh
openGauss-Server-6.0.5-openEuler22.03-x86_64.tar.bz2

student@openGauss~$ sha256sum openGauss-Server-6.0.5-openEuler22.03-x86_64.tar.bz2
```
### Step2 - Hardware and Software Requirements

Table - **Hardware Requirements**
| Item    | Minimum Requirement |
|---------|---------------------|
| CPU     | 2 cores             |
| Memory  | 4 GB                |
| Storage | 20 GB               |
| Network | 300 Mbit/s Ethernet |

```shell
$ lscpu
$ cat /proc/cpuinfo
```

```shell
$ free -h
$ cat /proc/meminfo | grep MemTotal
```

```shell
$ df -h /
$ lsblk
```

```shell
$ ip a
$ ethtool ens32
Speed: 1000Mb/s
```

Table - **Software Requirements**
| Software          | Description         |
|-------------------|---------------------|
| Linux OS          | openEuler 22.03 LTS |
| Linux file system | inode 1.5 billion   |
| Tool              | bzip2               |
| Python            | Python 3.6 or later |

```shell
$ uname -sr
$ cat /etc/system-release
```

```shell
$ df -i  
$ mkfs.ext4 -N 1500000000 /dev/sdb1  
```

```shell
$ rpm -q bzip2
$ python3 --version
```

### Step3 - installing Dependency Packages

Table - **Software Dependency Requirements**
| Software       | Recommended Version |
|----------------|---------------------|
| libaio-devel   | 0.3.109-13          |
| readline-devel | 7.0-13              |
| expect         | -                   |

```shell
student@openGauss~$ dnf clean all
student@openGauss~$ dnf makecache
```

```shell
# install the dependency packages
student@openGauss~$ sudo dnf install -y libaio-devel readline-devel expect net-tools tar
```

```shell
student@openGauss~$ rpm -q libaio-devel readline-devel expect
```

### Step4 - Disabling SELinux and firewalld

```shell
student@openGauss~$ sudo vi /etc/selinux/config
SELINUX=disabled
:wq

student@openGauss~$ sudo reboot

student@openGauss~$ sestatus
SELinux status: disabled
```

```shell
# Disable the firewall service
student@openGauss~$ systemctl disable firewalld
student@openGauss~$ sudo systemctl is-enabled firewalld

# Stop the firewall service
student@openGauss~$ sudo systemctl stop firewalld
student@openGauss~$ sudo systemctl status firewalld
```

### Step5 - Setting Character Set Parameters

```shell
student@openGauss~$ locale

student@openGauss~$ localectl set-locale LANG=en_US.UTF-8
student@openGauss~$ echo 'export LANG=en_US.UTF-8' >> /etc/profile
student@openGauss~$ source /etc/profile

student@openGauss~$ locale
LANG=en_US.UTF-8
```

### Step6 - Setting Time Zone and System Clock

```shell
student@openGauss~$ sudo cp /usr/share/zoneinfo/$Locale/$Time zone /etc/localtime
student@openGauss~$ date -s "Sat Sep 27 16:00:07 CST 2020"
```

### Step7 - Disabling Swap Memory (Optional)

```shell
student@openGauss~$ free -h
student@openGauss~$ swapon --show
student@openGauss~$ cat /proc/swaps
```

```shell
Disable (temporary) the Swap Memory 
student@openGauss~$ sudo swapoff -a

Enable the Swap Memory
student@openGauss~$ sudo swapon -a
```

### Step8 - Disabling RemoveIPC

```shell
student@openGauss~$ loginctl show-session | grep RemoveIPC
RemoveIPC=no

student@openGauss~$ systemctl show systemd-logind | grep RemoveIPC
RemoveIPC=no
```

### Step9 - Disabling History Command

```shell
student@openGauss~$ sudo vi /etc/profile
student@openGauss~$ source /etc/profile

student@openGauss~$ echo $HISTSIZE
0
```
