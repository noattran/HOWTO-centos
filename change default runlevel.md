In CentOS 7 / RHE 7,  systemd uses “targets” instead of run-levels.  /etc/inittab is no more used by systemd to change the run levels. This guide will help you to set up default runlevel in CentOS 7 / RHEL 7.
Default runlevel can be set either by using the systemctl command or making symbolic link of runlevel targets to default target file.
# Method 1:
Lets check the current run level by issuing the following command.
```
systemctl get-default
runlevel5.target
```
Before changing the default runlevel, we have to check out the available targets.
```
$systemctl list-units --type=target
```
Output will look like below.
```
UNIT                LOAD   ACTIVE SUB    DESCRIPTION
basic.target        loaded active active Basic System
cryptsetup.target   loaded active active Encrypted Volumes
getty.target        loaded active active Login Prompts
graphical.target    loaded active active Graphical Interface
local-fs-pre.target loaded active active Local File Systems (Pre)
local-fs.target     loaded active active Local File Systems
multi-user.target   loaded active active Multi-User System
network.target      loaded active active Network
nfs.target          loaded active active Network File System Server
paths.target        loaded active active Paths
remote-fs.target    loaded active active Remote File Systems
slices.target       loaded active active Slices
sockets.target      loaded active active Sockets
swap.target         loaded active active Swap
sysinit.target      loaded active active System Initialization
timers.target       loaded active active Timers
 
LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.
```
Change default to runlevel 3 (nothing but a multi-user.target).
```
$systemctl set-default multi-user.target
```
Confirm the default runlevel.
```
$systemctl get-default
multi-user.target
```
Reboot and check it out.
```
reboot
```
# Method 2:
You may noticed the similar output when the systemctl set-default multi-user.target command is issued. What the command is done is nothing but making  symabolic link of runlevel targets to the default target file.
```
rm '/etc/systemd/system/default.target'
ln -s '/usr/lib/systemd/system/multi-user.target' '/etc/systemd/system/default.target'
```
Check the current level.
```
$systemctl get-default
multi-user.target
```
Before making the symbolic link, lets list out the files in the systemd directory.
```$ls /lib/systemd/system/runlevel*target -l
```
Output will look like below.
```
lrwxrwxrwx. 1 root root 15 Aug  3 13:44 /lib/systemd/system/runlevel0.target -> poweroff.target
lrwxrwxrwx. 1 root root 13 Aug  3 13:44 /lib/systemd/system/runlevel1.target -> rescue.target
lrwxrwxrwx. 1 root root 17 Aug  3 13:44 /lib/systemd/system/runlevel2.target -> multi-user.target
lrwxrwxrwx. 1 root root 17 Aug  3 13:44 /lib/systemd/system/runlevel3.target -> multi-user.target
lrwxrwxrwx. 1 root root 17 Aug  3 13:44 /lib/systemd/system/runlevel4.target -> multi-user.target
lrwxrwxrwx. 1 root root 16 Aug  3 13:44 /lib/systemd/system/runlevel5.target -> graphical.target
lrwxrwxrwx. 1 root root 13 Aug  3 13:44 /lib/systemd/system/runlevel6.target -> reboot.target
```
As per the previous step, current default run level 3. Issue the following command to make symbolic link of runlevel5.target to default.target file.
```
$ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
```
or
```
$ln -sf /lib/systemd/system/graphical.target /etc/systemd/system/default.target
```
Again check the current level.
```$systemctl get-default
runlevel5.target
```
Now the default runlevel is 5 (graphical mode), reboot the server and check it out.
```
$reboot
```
That’s All!., hope this helped you, we welcome your comments.
