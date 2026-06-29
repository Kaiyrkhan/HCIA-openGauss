# Installation & Deployment

## Preparing the Installation Environment

### Step1 - Obtaining the Installation Package

**Download Link:** https://opengauss.org/en/download/  

```shell
$ tar -jxf openGauss-All-6.0.5-openEuler22.03-x86_64.tar.gz
$ ls -lb
```
### Step1 - Hardware and Software Requirements

Table1 - **Hardware Requirements**
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

Table2 - **Software Requirements**
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
