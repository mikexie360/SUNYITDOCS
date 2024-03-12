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

This will setup firewall & Port forwarding so the DB is reachable by the host machine

## Send JDK, database rpm and weblogic to the oracle virtual machine.

use winscp to send the jdk, oracle db and weblogic to your virtual machine.

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

We need to create a tnsnames.ora file on our local machine.
in your C drive on your local machine, create the following folders and files

C:/suny/oracle/tnsnames.ora

and this file will include the definition for your local database

We need to add the following to our tnsnames.ora file.

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

**However, we might actually have tnsnames.org file for you. so you might not have to create one or edit one.**

## Set up the database to start automatically after a reboot

The oratab file is used to say which database should be brought up at boot so we want to change the ORCLCDB or whatever default to Y in:

use the cat command to read it to see if there is a Y at the end

```
cat /etc/oratab
```

use vim or some other text editor to edit the file so that it shows up as 
ORCLCDB:/opt/oracle/product/19c/dbhome_1:Y

you can use nano or vim

```
vim /etc/oratab
```

## creating a service or oracle_database

Then we create a service for oracle_database:

run this command to create the service

```
gedit /etc/systemd/system/oracle_database.service
```

you can use cygwin or some other way to paste the following text easier.

```
#/etc/systemd/system/oracle_database.service
#Starting oracle Databases up at boot.
#This starts the databases in /etc/oratab with a third : of ‘Y’

[Unit]
Description=The Oracle Database Service
After=network.target

[Service]
Type=forking
RemainAfterExit=yes
KillMode=none
TimeoutStopSec=10min
# memlock limit is needed for SGA to use HugePages
LimitMEMLOCK=infinity
LimitNOFILE=65535

User=oracle
Group=oracle
# Please use absolute path here
# ExecStart=$ORACLE_HOME/bin/dbstart $ORACLE_HOME &
# First argument of dbstart is used to bring up Listener
#export ORACLE_HOME=/opt/oracle/product/19c/dbhome_1 
#dbstart

ExecStart=/opt/oracle/product/19c/dbhome_1/bin/dbstart /opt/oracle/product/19c/dbhome_1 &
ExecStop=/opt/oracle/product/19c/dbhome_1/bin/dbshut /opt/oracle/product/19c/dbhome_1
Restart=no

[Install]
# Puts wants directive for the other units in the relationship

WantedBy=default.target
```

Use the following command to start it up

```
systemctl start oracle_database
```


```
systemctl status oracle_database
```

Use the following command to stop it.

```
systemctl stop oracle_database
```

## Set up weblogic

To run your weblogic, you need java, and to get java, you need your jdk

Make sure your JDK file is ungzipped, untar, and unzipped and on the virtual machine.

to install weblogic use the following command on your virtual machine or cygwin that is connected to your virtual machine.

```
java -jar fmw_14.1.1.0.0_wls_lite_generic.jar
```

the java is from the jdk that you unzipped, you might have to update the paths or enter the correct path along with it.

fmw_14.1.1.0.0_wls_lite_generic.jar is the weblogic installer.

install weblogic, using the defaults

to run web logic run the following
```./startWebLogic.sh```

## Eclipse set up

### Try and install the correct version of eclipse.
[Try out this version of eclipse ee if no one else tells you what version you should use yet](https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2023-06/R/eclipse-jee-2023-06-R-win32-x86_64.zip)

You may have to install many different versions of eclipse ee, as many teams may have many different versions of eclipse being used.
So create a folder that keeps and stores all the different versions of eclipse that you use.

Once you have eclipse installed, go to help->Install new software...
And then copy and paste the below url into the work with input field and hit next.
https://subclipse.github.io/updates/
it will give a bunch of options, just select everything and install it, we can whittle it down later if needed.
let it install and eclipse might restart to finish installing.
It takes a while for eclipse to install this.

After it installs and restarts eclipse going to the perspective for “SVN Repositories Exploring” you should now be able to do a ‘New’>’Repository Location’ and add in: http://svn.sysadmin.suny.edu:9999/svn/sunydevrepo
