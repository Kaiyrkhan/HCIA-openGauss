# Lab Environment

**Download VMware Desktop Hypervisor** https://drive.google.com/drive/folders/1xPeOKfdeOzGdEHJRhYktJThgL6-xjkHy?usp=sharing  

**Download openEuler 22.03 LTS SP4** https://www.openeuler.org/en/download/?archive=true  

Table - **System Requirements**
| Component | Minimum Requirement |
|-----------|---------------------|
| Memory    | 4 GB                |
| Processor | 2 cores             |
| Storage   | 20 GB               |

![images](./images/vmware_vm_config_type_custom.png)  
![images](./images/vmware_create_blank_hard_disk.png)  
![images](./images/vmware_guest_os_opengauss.png)  
![images](./images/vmware_vm_name_opengauss.png)  
![images](./images/vmware_cpu.png)  
![images](./images/vmware_ram_opengauss.png)  
![images](./images/vmware_network_connection.png)  
![images](./images/vmware_scsi_controller.png)  
![images](./images/vmware_virtual_disk_type.png)  
![images](./images/vmware_create_new_virtual_disk.png)  
![images](./images/vmware_virtual_disk_size_opengauss.png)  
![images](./images/vmware_virtual_disk_file_name_opengauss.png)  
![images](./images/vmware_cdrom_opengauss.png)  
![images](./images/openEuler_boot_menu.png)  
![images](./images/openEuler_installation_process_language.png)  
![images](./images/openEuler_installation_summary.png)  
![images](./images/openEuler_timezone.png)  
![images](./images/openEuler_storage.png)  
![images](./images/openEuler_network_hostname.png)  
![images](./images/openEuler_root_account.png)  
![images](./images/openEuler_create_user.png)  
![images](./images/openEuler_begin_installation.png)  
![images](./images/openEuler_install_complete.png)  

**Shut Down Guest**  
![images](./images/vmware_shutdown_vm.png)  

**Hardware Device (RAM, CPU, Storage, NIC, Display)**  
![images](./images/openEuler_hardware_devices.png)  

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
