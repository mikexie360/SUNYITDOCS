# Overview of the tools you need

You need to install winscp, cygwin and Eclipse EE

Oracle VM, PLSQL Developer is already installed

WebLogic, JDK, Oracle Database rpm, and linux iso files need to be downloaded over winscp.

## Winscp

Winscp will allow you to connect to your vm or other people's vm to send or recive files.

[Can be downloaded here](https://winscp.net/eng/index.php)

Might be needed to download 4 other files from someones VM0

- Oracle Linux ISO
- Oracle database rpm
- JDK
- Weblogic

## Cygwin

Cygwin will allow you to use commandline to control your vm, and start up weblogic.

[install cygwin here](https://www.cygwin.com/install.html)

## Oracle VM

Oracle VM is already installed on your machine.


## Oracle Linux iso
You will have to get the Oracle LInux ISO from someone else through winscp

## Oracle database rpm
You will have to get the Oracle database rpm from someone else through winscp

## JDK
Do not install java or jdk through an exe file. Install JDKs for specifc versions that you need, and make sure you have a folder for all versions of JDKs. The correct version of the JDK you need should be in winscp from someone else's virtual machine.

## WebLogic
The files will need to be downloaded from winscp from someone else's machine.

## Eclpise EE
A link to the correct version of eclipse ee should be provided to you.
You may need a folder of the different versions of eclipse ee.

[Download eclipse ee here, but it might not be the correct version](https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2023-06/R/eclipse-jee-2023-06-R-win32-x86_64.zip)

## PLSQL Developer
PLSQL Developer should be already downloaded in your local machine.

## General Overview
Weblogic will be ran on the server by a java jdk in cygwin connected to the virtual machine you set up, running both weblogic and the database, with eclipse and PLSQL Developer on the local machine.
