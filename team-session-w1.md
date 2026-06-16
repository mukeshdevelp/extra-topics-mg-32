# Linux Fundamentals & Shell Scripting – Complete Learning Guide

## Table of Contents

1. [Linux Basic Commands](#1-linux-basic-commands)
2. [Linux Directory Structure](#2-linux-directory-structure)
3. [User Management & File Permissions](#3-user-management--file-permissions)
4. [Bash Scripting - Conditionals, Variables, Getopts](#4-bash-scripting---conditionals-variables-getopts)
5. [Package Management](#5-package-management)

---

# 1. Linux Basic Commands

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Linux commands allow users to interact with the operating system through CLI.

Architecture:

```text
User
 ↓
Shell (Bash)
 ↓
Kernel
 ↓
Hardware
```

---

## Navigation Commands

Current directory:

```bash
pwd
```

List files:

```bash
ls
```

Detailed list:

```bash
ls -la
```

Move directory:

```bash
cd /path
```

Go back:

```bash
cd ..
```

Go home:

```bash
cd ~
```

---

## File Operations

Create file:

```bash
touch file.txt
```

Create directory:

```bash
mkdir project
```

Copy:

```bash
cp file1 file2
```

Move:

```bash
mv old.txt new.txt
```

Delete:

```bash
rm file.txt
```

Delete directory:

```bash
rm -rf folder
```

---

## Search Commands

Find file:

```bash
find . -name "*.txt"
```

Search text:

```bash
grep nginx file.txt
```

Locate:

```bash
locate nginx
```

---

## System Commands

Current user:

```bash
whoami
```

Check memory:

```bash
free -h
```

Disk usage:

```bash
df -h
```

Processes:

```bash
ps aux
```

Live monitoring:

```bash
top
```

---

## Layman Explanation

Linux commands are remote controls for your operating system.

---

# 2. Linux Directory Structure

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Linux uses a hierarchical filesystem.

Structure:

```text
/

├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── opt
├── proc
├── root
├── tmp
├── usr
└── var
```

---

## Important Directories

### /

Root directory.

---

### /home

User files.

Example:

```text
/home/mukesh
```

---

### /etc

Configuration files.

Example:

```text
/etc/ssh/sshd_config
```

---

### /var

Logs and variable data.

```text
/var/log
```

---

### /tmp

Temporary files.

---

### /usr

Installed applications.

---

### /proc

Runtime process info.

---

## Commands

View structure:

```bash
tree /
```

Disk usage:

```bash
du -sh *
```

---

## Layman Explanation

Linux directories are like rooms in a house.

---

# 3. User Management & File Permissions

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Linux controls access using users, groups, and permissions.

Permission Model:

```text
Owner
Group
Others
```

Permission Values:

```text
r = 4
w = 2
x = 1
```

Example:

```text
755
```

Meaning:

```text
Owner → rwx
Group → r-x
Other → r-x
```

---

## User Management

Create user:

```bash
sudo useradd devuser
```

Set password:

```bash
sudo passwd devuser
```

Delete:

```bash
sudo userdel devuser
```

Groups:

```bash
groups
```

Add user:

```bash
sudo usermod -aG docker devuser
```

---

## File Permissions

View:

```bash
ls -l
```

Change permission:

```bash
chmod 755 script.sh
```

Change ownership:

```bash
sudo chown ubuntu:ubuntu app.txt
```

Special Permissions:

```text
SUID
SGID
Sticky Bit
```

---

## Layman Explanation

Permissions are security guards deciding who can enter.

---

# 4. Bash Scripting - Conditionals, Variables, Getopts

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Bash scripting automates command execution.

---

## Variables

Create:

```bash
NAME="Linux"
```

Use:

```bash
echo $NAME
```

---

## User Input

```bash
read username

echo $username
```

---

## Conditionals

Example:

```bash
if [ $USER == "ubuntu" ]
then
echo "valid"
else
echo "invalid"
fi
```

---

## Loops

For loop:

```bash
for i in 1 2 3
do
echo $i
done
```

While:

```bash
while true
do
echo running
done
```

---

## Functions

```bash
hello(){

echo Hi

}

hello
```

---

## Getopts

Accept flags.

Example:

```bash
while getopts n:a: flag
do

case "${flag}" in

n) NAME=$OPTARG;;

a) AGE=$OPTARG;;

esac

done
```

Run:

```bash
./script.sh -n Mukesh -a 25
```

---

## Example Script

```bash
#!/bin/bash

if [ $1 == "start" ]

then

echo Starting

fi
```

---

## Layman Explanation

Shell script = Record commands once and replay forever.

---

# 5. Package Management

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Package managers install and manage software.

Flow:

```text
Repository
↓
Package Manager
↓
Install
```

---

## APT (Ubuntu)

Update:

```bash
sudo apt update
```

Install:

```bash
sudo apt install nginx
```

Remove:

```bash
sudo apt remove nginx
```

Search:

```bash
apt search nginx
```

---

## YUM / DNF (RHEL)

Install:

```bash
sudo yum install nginx
```

Update:

```bash
sudo yum update
```

---

## Snap

Install:

```bash
sudo snap install code
```

---

## DPKG

Install:

```bash
sudo dpkg -i package.deb
```

---

## RPM

Install:

```bash
sudo rpm -ivh package.rpm
```

---

## Verify Packages

List:

```bash
dpkg -l
```

Check:

```bash
rpm -qa
```

---

## Layman Explanation

Package managers are app stores for Linux.

---

# Summary

This guide covered:

✅ Linux Commands
✅ Directory Structure
✅ Users & Permissions
✅ Bash Scripting
✅ Package Management

Happy Learning 🚀
