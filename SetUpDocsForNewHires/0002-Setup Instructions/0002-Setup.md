# Set up instructions

Make sure you have everything downloaded

We need to download the oracle db, the OS iso, and the database rpm, and the zip of the jdk by using winscp

You will need to connect to this ip address through winscp

IPv4 Address. . . . . . . . . . . : 10.141.27.57
username and password is root/Welcome1!2345

the directory /home/oracle/shared will have the files

Then set up your virtual machine with the oracle iso
Make sure the all the passwords in your virtual machine and database is Welcome1

Here are the reconmended specs and set up for your virtual machine.

## Virtual Machine

hard drive: 220.00 GB
Memory: 10240 M
Processors: more than one
Use Host I/O Cache: (Enabled on all storage controllers)
Pointing Device: USB Tablet (Only used on install, then interact with SSH -X)
Port Forwarding -> (Also setup the OS’s) Firewall:

- SSH: 25 -> 22
- Database: 1521 -> 1521
- Enterprise Manager: 5500 -> 5500
- Enterprise Manager: 7001 -> 7001

## OS Configuration

Software Selection: Server with GUI (no sub selections)
Installation Destination: (Custom) ‘I will configure partitioning’:
/home Desired Capacity: 58 GiB (Shrink)
Swap Desired Capacity: 12 GiB (Expand) – required by Database.
Done, Accept Changes
Network & Host Name:
    Off -> On
    Configure
    General -> ‘Automatically connect to this network when it is available’

Begin Installation…. Then while installing:
Set root password
  Password reconmendation is **Welcome1**
Create a user account: Oracle
  Make sure the user account is not set as an admin
  the user account password can also be **Welcome1**
When finished: Click Reboot.
Finish OS Install:
Accept Licensing Information

## Finish configuration

you may have to switch to the root account.
You can do this with the su command to switch between accounts

To switch to admin use this command and then type in the root password

```
su
```

To switch back to the oracle user account use this command

```
su oracle
```

Finish Configuration
Ensure ports are unblocked on the firewall.

```
firewall-cmd --permanent --add-port=22/tcp
firewall-cmd --permanent --add-port=1521/tcp
firewall-cmd --reload
```

## Install the database 19c as root

Make sure you transfered the database rpm through winscp.
use su command to switch to root
```su```

```
yum install -y oracle-database-preinstall-19c
rpm -i oracle-database-ee-19c-1.0-1.x86_64.rpm
/etc/init.d/oracledb_ORCLCDB-19c configure
```

Go back to the user account and set the oracle home and the SID of the database.

```
export ORACLE_HOME=/opt/oracle/product/19c/dbhome_1
export PATH=$PATH:$ORACLE_HOME/bin
export ORACLE_SID=ORCLCDB
```

We need to get into sqlplus to set the passwords for sys and system.

```
/opt/oracle/product/19c/dbhome_1/bin/sqlplus / as sysdba
```

you can use PL/SQL Developer to add these users
```
ALTER USER SYS IDENTIFIED BY Welcome1;
ALTER USER SYSTEM IDENTIFIED BY Welcome1;
```

We need to add the following to our tnsnames.org.

```
OIF19.world =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = ORCLCDB)
    )
  )
```

use winscp to send the jdk, oracle db and weblogic to your virtual machine.
