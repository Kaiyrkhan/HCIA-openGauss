# Lab Environment

**Download VMware Desktop Hypervisor** https://drive.google.com/drive/folders/1xPeOKfdeOzGdEHJRhYktJThgL6-xjkHy?usp=sharing  

![images](./images/vmware_custom.png)  
![images](./images/vmware_blank.png)  
![images](./images/vmware_guest_os.png)  
![images](./images/vmware_vm_name.png)  
![images](./images/vmware_cpu.png)  
![images](./images/vmware_ram.png)  
![images](./images/vmware_scsi_controller.png)  
![images](./images/vmware_virtual_disk_type.png)  
![images](./images/vmware_create_disk.png)  
![images](./images/vmware_disk_size.png)  
![images](./images/vmware_disk_file_name.png)  
![images](./images/vmware_cdrom.png)  
![images](./images/openEuler_install_menu.png)  
![images](./images/openEuler_install_language.png)  
![images](./images/openEuler_configure_ethernet.png)  
![images](./images/openEuler_configure_timezone.png)  
![images](./images/openEuler_configure_root_account.png)  
![images](./images/openEuler_configure_user_account.png)  
![images](./images/openEuler_settings_menu.png)  
![images](./images/openEuler_install_complete.png)  
![images](./images/vmware_vm_devices.png)  

login: **student**  
password: **123**  

```shell
student@openGauss~$ sudo passwd student
New password: 123

student@openGauss~$ sudo passwd root
New password: P@s$w0rd
```

```shell
student@openGauss~$ ping google.com -c2
```

```shell
student@openGauss~$ sudo dnf clean all
student@openGauss~$ sudo dnf makecache

student@openGauss~$ sudo dnf update -y
```

```shell
student@openGauss~$ sudo reboot
```

```shell
student@openGauss~$ sudo systemctl status sshd

student@openGauss~$ ip address
```

Configure Console Login Banner
```shell
student@openGauss~$ sudo vi /etc/issue
\S \l
Kernel \r

******************************************
Username: omm
Password: 123
******************************************
ENTER
ENTER

:wq
```

Clear Bash History
```shell
student@openGauss~$ history

student@openGauss~$ ls -la
student@openGauss~$ cat /dev/null > ~/.bash_history
student@openGauss~$ history -c
```
