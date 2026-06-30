# Preparing the Installation Environment

### Step1 - Obtaining installation Package

**Download Link:** https://opengauss.org/en/download/  

```shell
$ ls -lh
openGauss-All-6.0.5-openEuler22.03-x86_64.tar.gz
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
df -i  
mkfs.ext4 -N 1500000000 /dev/sdb1  
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
$ dnf clean all
$ dnf makecache
```

```shell
# install the dependency packages
$ sudo dnf install -y libaio-devel readline-devel expect net-tools
```

```shell
$ rpm -q libaio-devel readline-devel expect
```

### Step4 - Disabling SELinux and firewalld

```shell
$ sudo vi /etc/selinux/config
SELINUX=disabled
:wq

$ sudo reboot
```

```shell
# Disable the firewall service
$ systemctl disable firewalld
$ sudo systemctl is-enabled firewalld

# Stop the firewall service
$ sudo systemctl stop firewalld
$ sudo systemctl status firewalld
```

### Step5 - Setting Character Set Parameters

```shell
$ locale

$ localectl set-locale LANG=en_US.UTF-8
$ echo 'export LANG=en_US.UTF-8' >> /etc/profile
$ source /etc/profile

$ locale
LANG=en_US.UTF-8
```

### Step6 - Setting Time Zone and System Clock

```shell
$ sudo cp /usr/share/zoneinfo/$Locale/$Time zone /etc/localtime
$ date -s "Sat Sep 27 16:00:07 CST 2020"
```

### Step7 - Disabling Swap Memory (Optional)

```shell
$ free -h
$ swapon --show
$ cat /proc/swaps
```

```shell
Disable (temporary) the Swap Memory 
$ sudo swapoff -a

Enable the Swap Memory
$ sudo swapon -a
```

### Step8 - Disabling RemoveIPC

```shell
$ loginctl show-session | grep RemoveIPC
RemoveIPC=no

$ systemctl show systemd-logind | grep RemoveIPC
RemoveIPC=no
```

### Step9 - Disabling History Command

```shell
$ sudo vi /etc/profile
$ source /etc/profile

$  echo $HISTSIZE
0
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
