CentOS 7 supports three class of Host Names:

**_Static_** – The static host name is traditional host which can be chosen by the user and is stored in _/etc/hostname_ file.
**_Transient_** – The transient host name is maintained by kernel and can be changed by DHCP and mDNS.<br>
**_Pretty_** – It is a free form UTF -8 host name for the presentation to the user.

HostName can be,
- 64 character in a length
- Recommend to have FQDN
- Consists of a-z,A-Z, 0-9, “-”, “_” and “.” only
# How to Change:
Before changing the host name, lets check the current host name.
```
[root@localhost ~]# hostname
localhost.localdomain
```
## 1. nmtui tool: 
NetworkManaget tool is used to set the static host name in /etc/hostname file.
<p align="center">
<img src="https://user-images.githubusercontent.com/13705439/62769517-58a35900-bac3-11e9-9e9e-c4331b175286.png"><br>
nmtui -Select Set HostName
</p>
Set the host name.
<p align="center">
<img src="https://user-images.githubusercontent.com/13705439/62769522-5b05b300-bac3-11e9-9d12-48b6e81df22b.png"><br>
nmtui – Change HostName 2
</p>

restart the hostnamed to force the hostnamectl to notice the change in static host name.
```
[root@localhost ~]# systemctl restart systemd-hostnamed
```
You can verify the change in host name.
```[root@server ~]# hostname
server.itzgeek.com
[root@server ~]# cat /etc/hostname
server.itzgeek.com
[root@server ~]# cat /etc/sysconfig/network
#Created by anaconda
HOSTNAME=server.itzgeek.com
```
## 2. hostnamectl: 
hostnamectl is used to change the host name, with this tool we can change all the three class of host name; here we look only static host name.
Check the current host name.
```
[root@server ~]# hostnamectl status
Static hostname: server.itzgeek.com
Icon name: computer-vm
Chassis: vm
Machine ID: 565ea8b749544aca9d5563308f9e4bc2
Boot ID: 5c979d9b5f754df8b75a4e3aeabf2bad
Virtualization: vmware
Operating System: CentOS Linux 7 (Core)
CPE OS Name: cpe:/o:centos:centos:7
Kernel: Linux 3.10.0-123.el7.x86_64
Architecture: x86_64
```
Set the hostname.
```
[root@server ~]# hostnamectl set-hostname client.itzgeek.com
```
Check the host name again (Close the session and open new session using putty or console)
```
[root@client ~]# hostnamectl status
Static hostname: client.itzgeek.com
Icon name: computer-vm
Chassis: vm
Machine ID: 565ea8b749544aca9d5563308f9e4bc2
Boot ID: 5c979d9b5f754df8b75a4e3aeabf2bad
Virtualization: vmware
Operating System: CentOS Linux 7 (Core)
CPE OS Name: cpe:/o:centos:centos:7
Kernel: Linux 3.10.0-123.el7.x86_64
Architecture: x86_64
```
If you use this command, you do not require to notify the change in host name. Close the current session and re launch the terminal.
## 3. nmcli tool: 
It can be used to query and setup the static host name in /etc/hostname file.
Check the hostname.
```
[root@client ~]# nmcli general hostname
client.itzgeek.com
```
Change the host name.
```
[root@client ~]# nmcli general hostname server.itzgeek.com
```
restart the hostnamed to force the hostnamectl to notice the change in static host name.
```
[root@client ~]# systemctl restart systemd-hostnamed
```
## 4. Edit /etc/hostname
This is the simple, but requires a reboot of server to take an effect.
Note: Use the hostnamectl to change the host name, which fair better than other commands and does not require to update the kernel about the change in host name.
