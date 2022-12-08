# Put Sonar on Linux Locally

## **Table of Contents**
**[1. Tools & Local Environment](#1-tools--local-environment)**

* [1.1 Preparation Steps](#11-preparation-steps)
* [1.2 Virtual Ubuntu](#12-virtual-ubuntu)
* [1.3 Virtual CentOS](#13-virtual-centos)
* [1.4 Getting Linux Setup for Development](#14-getting-linux-setup-for-development)
    * [1.4.1 Installing Git](#141-installing-git)
    * [1.4.2 Other Tools](#142-other-tools)

**[2 Testing Sonar on your Linux](#2-testing-sonar-on-your-linux)**

**[3 Sonar Notes on AWS](#3-sonar-notes-on-aws)**

**[4 Random Links and Notes](#4-random-links-and-notes)**

## **1. Tools & Local Environment**

This guide will be assuming you have admin permissions and are working on a personal machine or something intended for development.

You'll need some tools:
 1. a text editor ( visual studio code, vim, notepad++, sublime text, etc.)
 2. a way to setup local servers (virtual box is free-99)

Since we are heading towards using Sonarqube in linux, you'll want a linux environment. Virtualbox is a good way to get that stood up on Windows/Mac. There are other ways (WSL, cygwin, mingw, parallels, dual booting a personal machine) but Virtualbox should work on your machine and is easily changed-destroyed-rebuilt without taking much time. (Docker is good too, but that's for later)

### **1.1 preparation steps**
1. make a folder in your Documents (or wherever) called "Sonar Local"
    * this will be the "catch-all" folder for this project.

### **1.2 Virtual Ubuntu**

You can get virtual box from https://www.virtualbox.org/wiki/Downloads
1. Under the "platform packages" section, you'll need the "Windows Hosts" or "OS X Hosts" depending on your operating system
    * I'll be using windows for mine, but if you're on a mac it should be close enough to follow along. If it's not, we can make this document better.
1. This should prompt you to save the file. Put it in your local sonar folder and once it is done downloading, run it.
1. Just do the default install options. Once it's installed you'll see something like this
    * ![install complete screen](Screenshots/VirtualBoxInstallComplete.png)
1. Now you'll need a linux image. I'm going to use Ubuntu LTS for this guide. You can get it from [Ubuntu's downloads page](https://ubuntu.com/download/desktop).
1. Once that is downloaded, start VirtualBox and go to Machine>New. Give your machine 8Gb(8192mb) memory and name it Ubuntu LTS. It should match the screen below.
    * ![Ubuntu Virtual figure 1](Screenshots/VirtualBoxUbuntuLTS1.png)
1. Now you'll create the hard disk. I'm going with 40GB, but we may not need this much for Sonar and our toolchain. It should match the screen below.
    * ![Ubuntu Virtual figure 2](Screenshots/VirtualBoxUbuntuLTS2.png)
1. Now click "Start" in VirtualBox Manager. You should get prompted to select an image to boot, but if not...
    1. go to "Devices">"Optical Drives">"Choose a disk file..."
        * ![Ubuntu Virtual figure 3](Screenshots/VirtualBoxUbuntuLTS3.png)
        * ![Ubuntu Virtual figure 4](Screenshots/VirtualBoxUbuntuLTS4.png)
    1. Use the "Add" button and select your Ubuntu LTS iso file from earlier.
        * ![Ubuntu Virtual figure 5](Screenshots/VirtualBoxUbuntuLTS5.png)
    1. You may need to reset the machine. Use "Machine">"Reset.
        * ![Ubuntu Virtual figure 6](Screenshots/VirtualBoxUbuntuLTS6.png)
1. When it restarts, you should see the screen below. Use the arrow keys to select "*Try or Install Ubuntu" and press Enter.
    * ![Ubuntu Virtual figure 7](Screenshots/VirtualBoxUbuntuLTS7.png)
    * I *really* don't want to do a full How-To on installing Ubuntu because they've got a great one and why re-invent the wheel? Having said that, here are some notes/suggestions.
        * minimal installation will be more than enough for what we're going to do
            * ![Ubuntu Virtual figure 8](Screenshots/VirtualBoxUbuntuLTS8.png)
        * These 2 screens look scary, but Erase the disk (the virtual 40Gb one we just made) and hit "Continue" on the confirmation.
            * ![Ubuntu Virtual figure 9](Screenshots/VirtualBoxUbuntuLTS9.png)
            * ![Ubuntu Virtual figure 10](Screenshots/VirtualBoxUbuntuLTS10.png)
1. Now just go through the setup for Ubuntu and install the updates. You should default to "No" when it's asking about anonymous data or location or tracking or any of that stuff. :) (draw the rest of the owl jokes incoming)

### **1.3 Virtual Centos**

*VERY WORK-IN-PROGRESS*

Loose notes below:
1. You'll need CentOS Stream 9 downloaded from the [centos.org downloads page](https://www.centos.org/download/)
1. Give it like 8GB ram and 4 cores or you'll be watching a slideshow
1. Name it something like "CentOS Stream 9" and VirtualBox should set the template as RedHat linux.
1. If you're on Windows 10/11, make sure in "Turn Windows features on or off" you have "Virtual Machine Platform" UNCHECKED.
1. run this in terminal to make guest additions for linux work:
    * `$sudo yum install gcc kernel-devel kernel-headers make bzip2 perl`

### **1.4 Getting Linux setup for development**

Good news, we're on ubuntu and everything you need to know is already in a forum somewhere or documented by the Ubuntu team.
Bad news, our Ubuntu from above needs some stuff to be ready for everything we'll need to do.
Good news (abounds), getting stuff on linux is **SUPER *EASY*** because you just get it from the package manager.

#### **1.4.1 Installing Git**

apt/yum is a good way to get everything you need, including this document (hint hint).

1. Open a terminal in ubuntu (press the windows/super key and start typing terminal)
1. Congratulations, that's the most valuable trick in this whole section.
1. Type `sudo apt install git -y`.
    * ![Ubuntu Git Install 1](Screenshots/VirtualBoxUbuntuGitInstall1.png)
1. Now, type `git --version`.
    * ![Ubuntu Git Install 2](Screenshots/VirtualBoxUbuntuGitInstall2.png)

Now let's get the repo containing this document with the git install we've just completed.

Type the following steps:
* `$cd /home/<yourusername>/Documents`
* `$mkdir 'Sonar Local'`
* `$cd Sonar\ Local/`
* `$git clone https://github.com/mtthwmths/SonarVBLocal`
* `$cd SonarVBLocal`
* `$vim SettingUpALocalSonarOnLinux.md`

You're now staring at this document in your terminal's text editor 'Vim'.

#### **1.4.2 Other Tools**

I'm just gonna rattle off the stuff I'm using below here with any weird steps if they come up.
You should be able to just type `$sudo apt install <packagename>` or `$sudo yum install <packagename>` to get everything on this list.

* python3
* sed
* awk
* perl
* java-11-openjdk-devel

## **2 Testing Sonar on your Linux**

Loosely based on the [2 minute guide Sonarqube.org has](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/).

For this section, I will be working in my Ubuntu VirtualBox machine. If you went with a cooler solution, then these should still work but you'll need to adjust for other package management or hardware/software configurations.

1. download Sonarqube Community latest
1. install java-11
    * Ubuntu: `$sudo apt install java-11-openjdk-devel`
    * CentOS: `$sudo yum install java-11-openjdk-devel`
    * Amazon Linux 2: `$sudo amazon-linux-extras install java-openjdk11`
        * you can run `$sudo amazon-linux-extras` to check what is enabled from the amazon-linux-extras repository
1. unzip the Sonarqube Community archive
1. navigate to /bin/linux/
1. run this
    * `./sonar.sh console`
1. once the text in your terminal stops, open a browser and navigate to `http://localhost:9000`
    * user:pass should be admin:admin

postgres local install
notes on psql config
 - don't use my_schema
 -   sudo yum :
     - install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm 
     - install postgresql14-server

LVM expansion
 - first use parted
     - unit B print free
     - resizepart 2 \[whatever the end of the free space at the end is]
     - quit
 - Now use lvm
     - pvresize <pv-name>
     - lvresize /dev/<vol-group>/<lv-name> <pv-name>
     - quit
 - Finally use xfs_growfs /dev/<vol-group>/<lv-name> or resize2fs /dev/<vol-group>/<lv-name>

## **3 Sonar Notes on AWS**
adduser sonar
groupadd sonar
usermod -a -G sonar sonar
check that with $groups sonar

check java version with `$sudo yum list installed | grep java` and it should return the packages that are being tracked and updated by `$sudo yum update -y`

added line to /etc/fstab/ to persist the mounting of software drive in opt
    - `UUID=18ed6f7b-c83f-4d22-8f52-a3d8f7266eb9 /opt/Software/ xfs defaults,noatime 1 1`
    - UUID is from lsblk -fs /dev/sdb/

systemd/system/sonarqube.service file
 - set user and group and sonar
 - set java as /bin/java
 - set directory as /opt/Software/sonarqube
     - this implies that when upgrading/updating sonar we will make sure to unzip into /opt/Software/sonarqube
     - Because the sonar-application jar name ends with the version of SonarQube, you will need to adjust the ExecStart command accordingly on install and at each upgrade.
 - All SonarQube directories should be owned by the sonarqube user. (no really check them all)
     - `#chown -R sonar:sonar /opt/Software/sonarqube`

elasticsearch is hongry. run this to expand vm.max_blah
 - `sudo su && cd /etc/ && vim sysctl.conf` 
     - then add in the line below
     - vm.max_map_count=524288
then reload the sysctl configs using this
 - `sysctl --system`
then check that the changes took by using
 - `sysctl vm.max_map_count`

 quicknotes 11182022 (connecting to mssql db from 201)
 - copied config file from 205 sonar test to connect sonar linux to test db.
 - test db is currently an copy of prod sonar db from WE 11112022

 quicknotes 12082022 (connecting to rds and updating sonar from 9.6 -> 9.7)
 - had to wget the file from sonarqube binaries: `wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.7.1.62043.zip`
 - unzip, update sonarqube.service to point to the new folder, reboot or run the systemd update command.
     - it should prompt you with the command needed when you update the service file.
 - connecting to RDS
     - query: `ALTER DATABASE sonarcomm COLLATE SQL_Latin1_General_CP1_CS_AS`
     - connecting was endpoint,port and not endpoint:port
 - 

## **4 Random Links and Notes**
* [SecureCRT](https://www.vandyke.com/products/securecrt/index.html)
* [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [VS Code markdown doc](https://code.visualstudio.com/Docs/languages/markdown)
* [sqlserver2pgsql](https://github.com/dalibo/sqlserver2pgsql)
* [endpointdev article where I found sqlserver2pgsql](https://www.endpointdev.com/blog/2019/01/migrate-from-sql-server-to-postgresql/)
* [migrating sqlserver to postgres article](https://www.endpointdev.com/blog/2019/01/migrate-from-sql-server-to-postgresql/)
* [2 minute guide Sonarqube.org](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/)
* [sonar elastisearch vm-max-map-count](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/vm-max-map-count.html)
* [devopscube: sonar on linux](https://devopscube.com/setup-and-configure-sonarqube-on-linux/)
* [learnin about plsql schemas](https://dba.stackexchange.com/questions/106057/error-no-schema-has-been-selected-to-create-in)
* [plsql permissions doc](https://www.postgresql.org/docs/current/ddl-priv.html)
* [plsql peer auth failed](https://stackoverflow.com/questions/18664074/getting-error-peer-authentication-failed-for-user-postgres-when-trying-to-ge)
* [notes on lvm extension - tecmint](https://www.tecmint.com/extend-and-reduce-lvms-in-linux/)
* [stackoverflow about xfs_growfs](https://stackoverflow.com/questions/26305376/resize2fs-bad-magic-number-in-super-block-while-trying-to-open)
* [renamed a vg and can't boot](https://forums.centos.org/viewtopic.php?t=73033)
