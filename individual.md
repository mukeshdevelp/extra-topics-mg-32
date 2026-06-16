# Linux Sysadmin, Shell Scripting & Networking – Complete Learning Guide

## Table of Contents

### First Week Topics

1. [Sticky Bit](#1-sticky-bit)
2. [Cron](#2-cron)
3. [Shebang](#3-shebang)
4. [Locate](#4-locate)
5. [Link (Hard Link vs Soft Link)](#5-link-hard-link-vs-soft-link)
6. [Netstat](#6-netstat)
7. [Find](#7-find)
8. [Shell (sh vs bash)](#8-shell-sh-vs-bash)
9. [Setfacl](#9-setfacl)
10. [Inode](#10-inode)
11. [Ulimit](#11-ulimit)
12. [Umask](#12-umask)
13. [Top](#13-top)
14. [Positional Variables in Bash](#14-positional-variables-in-bash)
15. [Nano](#15-nano)
16. [Vim](#16-vim)
17. [Special Variables in Bash](#17-special-variables-in-bash)
18. [/etc/skel](#18-etcskel)
19. [Pipe](#19-pipe)
20. [Kill](#20-kill)
21. [Df Command](#21-df-command)
22. [Du Command](#22-du-command)

### Second Week Topics

23. [Regex](#23-regex)
24. [Sort & Uniq](#24-sort--uniq)
25. [Function](#25-function)
26. [Case](#26-case)
27. [Alias](#27-alias)
28. [Rsync](#28-rsync)
29. [Cut](#29-cut)
30. [Cmp](#30-cmp)
31. [Diff](#31-diff)
32. [Paste](#32-paste)
33. [Column](#33-column)
34. [Escape Character](#34-escape-character)
35. [Comm & Tr](#35-comm--tr)
36. [Scp](#36-scp)
37. [Environmental Variables](#37-environmental-variables)
38. [Ifconfig](#38-ifconfig)
39. [Git Basic Commands](#39-git-basic-commands)
40. [Truncate](#40-truncate)
41. [Swap Memory](#41-swap-memory)
42. [Nmap](#42-nmap)

---

# 1. Sticky Bit

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Sticky Bit prevents users from deleting files they do not own inside a shared directory.

Example:

```bash
chmod +t /shared
```

Verify:

```bash
ls -ld /shared
```

Output:

```text
drwxrwxrwt
```

---

## Layman Explanation

Apartment building where tenants cannot throw away others’ belongings.

---

# 2. Cron

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Cron schedules automated tasks.

Syntax:

```text
Minute Hour Day Month Week Command
```

Example:

```bash
crontab -e
```

```text
0 2 * * * backup.sh
```

View:

```bash
crontab -l
```

---

## Layman Explanation

Alarm clock for Linux.

---

# 3. Shebang

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Defines interpreter.

Examples:

```bash
#!/bin/bash
```

```python
#!/usr/bin/python3
```

Run:

```bash
chmod +x app.sh
./app.sh
```

---

## Layman Explanation

Instruction telling Linux which engine starts the script.

---

# 4. Locate

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Fast file search using database.

Search:

```bash
locate nginx
```

Refresh DB:

```bash
sudo updatedb
```

---

## Layman Explanation

Google search for local files.

---

# 5. Link (Hard Link vs Soft Link)

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Hard Link:

```bash
ln file hard
```

Soft Link:

```bash
ln -s file soft
```

Comparison:

| Feature       | Hard  | Soft      |
| ------------- | ----- | --------- |
| Inode         | Same  | Different |
| Cross FS      | No    | Yes       |
| Delete Source | Works | Breaks    |

---

## Layman Explanation

Hard → Twin copy
Soft → Shortcut

---

# 6. Netstat

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Shows connections.

Ports:

```bash
netstat -tulnp
```

Route:

```bash
netstat -rn
```

---

## Layman Explanation

Airport control tower.

---

# 7. Find

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Search files dynamically.

Examples:

```bash
find . -name "*.txt"
```

Delete:

```bash
find . -mtime +30
```

---

## Layman Explanation

Treasure hunt.

---

# 8. Shell (sh vs bash)

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Comparison:

| Feature    | sh      | bash     |
| ---------- | ------- | -------- |
| POSIX      | Yes     | Extended |
| Arrays     | No      | Yes      |
| Completion | Limited | Yes      |

Check:

```bash
echo $SHELL
```

---

## Layman Explanation

sh → Basic phone

bash → Smartphone

---

# 9. Setfacl

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Advanced permissions.

Grant:

```bash
setfacl -m u:user:rwx file
```

View:

```bash
getfacl file
```

---

## Layman Explanation

VIP access.

---

# 10. Inode

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Stores metadata.

View:

```bash
ls -i
```

Check:

```bash
df -i
```

---

## Layman Explanation

File ID card.

---

# 11. Ulimit

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Resource limits.

View:

```bash
ulimit -a
```

Set:

```bash
ulimit -n 65535
```

---

## Layman Explanation

Quota system.

---

# 12. Umask

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Default permissions.

Check:

```bash
umask
```

Set:

```bash
umask 022
```

Formula:

```text
777 - umask
```

---

# 13. Top

Monitor processes.

```bash
top
```

Alternative:

```bash
htop
```

---

# 14. Positional Variables in Bash

```bash
$0
$1
$2
$#
```

Example:

```bash
./script.sh dev 8080
```

---

# 15. Nano

Open:

```bash
nano file.txt
```

Save:

```text
CTRL+O
```

Exit:

```text
CTRL+X
```

---

# 16. Vim

Open:

```bash
vim file.txt
```

Modes:

```text
Normal
Insert
Command
```

Save:

```text
:wq
```

---

# 17. Special Variables in Bash

Examples:

```bash
$?
$$
$*
$@
$#
```

Check exit:

```bash
echo $?
```

---

# 18. /etc/skel

Template files.

Location:

```text
/etc/skel
```

Create user:

```bash
useradd dev
```

---

# 19. Pipe

Combine commands.

Example:

```bash
cat file | grep nginx
```

Multiple:

```bash
ps aux | grep java
```

---

## Layman Explanation

Factory conveyor belt.

---

# 20. Kill

Terminate process.

```bash
kill PID
```

Force:

```bash
kill -9 PID
```

Search:

```bash
ps aux
```

---

# 21. Df Command

Disk usage.

```bash
df -h
```

Filesystem:

```bash
df -Th
```

---

# 22. Du Command

Directory usage.

```bash
du -sh
```

Largest:

```bash
du -sh * | sort -h
```



---

# 23. Regex

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Regex (Regular Expression) is used to match patterns.

Examples:

```bash
grep "^dev" users.txt
```

Numbers:

```bash
grep "[0-9]"
```

Email:

```bash
grep "[a-z0-9._%+-]+@[a-z]+\.[a-z]{2,}"
```

---

## Layman Explanation

Regex = Search formula.

---

# 24. Sort & Uniq

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Sort data.

```bash
sort file.txt
```

Reverse:

```bash
sort -r
```

Unique:

```bash
uniq file.txt
```

Count:

```bash
uniq -c
```

Example:

```bash
sort users.txt | uniq
```

---

## Layman Explanation

Arrange and remove duplicates.

---

# 25. Function

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Reusable code block.

```bash
hello(){

echo Hello

}

hello
```

Argument:

```bash
greet(){

echo $1

}
```

---

## Layman Explanation

Function = Saved command.

---

# 26. Case

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Switch-case logic.

```bash
case $1 in

start)
echo start
;;

stop)
echo stop
;;

esac
```

Run:

```bash
./app.sh start
```

---

## Layman Explanation

Menu selection.

---

# 27. Alias

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Shortcut commands.

Create:

```bash
alias ll="ls -la"
```

View:

```bash
alias
```

Remove:

```bash
unalias ll
```

Permanent:

```bash
vim ~/.bashrc
```

---

## Layman Explanation

Nickname for commands.

---

# 28. Rsync

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Fast sync tool.

Copy:

```bash
rsync -av source target
```

Remote:

```bash
rsync -av app user@server:/tmp
```

Delete:

```bash
rsync -av --delete
```

---

## Layman Explanation

Smart copy.

---

# 29. Cut

Extract columns.

```bash
cut -d ":" -f1 /etc/passwd
```

Characters:

```bash
cut -c 1-5
```

---

# 30. Cmp

Compare files.

```bash
cmp file1 file2
```

Silent:

```bash
cmp -s
```

---

## Layman Explanation

Spot difference.

---

# 31. Diff

Show differences.

```bash
diff file1 file2
```

Side:

```bash
diff -y
```

Recursive:

```bash
diff -r
```

---

## Layman Explanation

Compare documents.

---

# 32. Paste

Merge files.

```bash
paste file1 file2
```

Custom:

```bash
paste -d ","
```

---

# 33. Column

Format output.

```bash
column -t data.txt
```

CSV:

```bash
column -s "," -t
```

---

## Layman Explanation

Convert text to table.

---

# 34. Escape Character

Special handling.

Examples:

```bash
\"
\$
\\
```

Example:

```bash
echo "Hello \$USER"
```

---

## Layman Explanation

Treat special text normally.

---

# 35. Comm & Tr

## Comm

Compare sorted files.

```bash
comm a.txt b.txt
```

---

## Tr

Translate text.

Uppercase:

```bash
tr a-z A-Z
```

Delete:

```bash
tr -d " "
```

---

## Layman Explanation

Comm = Compare

Tr = Transform

---

# 36. Scp

Secure copy.

Upload:

```bash
scp app ubuntu@server:/tmp
```

Download:

```bash
scp ubuntu@server:/tmp/app .
```

Recursive:

```bash
scp -r
```

---

## Layman Explanation

Copy files securely.

---

# 37. Environmental Variables

View:

```bash
env
```

Create:

```bash
export APP=prod
```

Use:

```bash
echo $APP
```

Permanent:

```bash
~/.bashrc
```

---

## Layman Explanation

Global settings.

---

# 38. Ifconfig

Network info.

Show:

```bash
ifconfig
```

Enable:

```bash
ifconfig eth0 up
```

Modern:

```bash
ip a
```

---

## Layman Explanation

Vehicle dashboard.

---

# 39. Git Basic Commands

Initialize:

```bash
git init
```

Clone:

```bash
git clone URL
```

Status:

```bash
git status
```

Commit:

```bash
git commit -m "msg"
```

Push:

```bash
git push
```

Pull:

```bash
git pull
```

Branch:

```bash
git branch
```

---

## Layman Explanation

Version history.

---

# 40. Truncate

Reduce file size.

Clear:

```bash
truncate -s 0 app.log
```

Size:

```bash
truncate -s 1M file
```

---

## Layman Explanation

Shrink file.

---

# 41. Swap Memory

Virtual RAM.

View:

```bash
free -h
```

Create:

```bash
fallocate -l 2G /swapfile
```

Enable:

```bash
mkswap /swapfile

swapon /swapfile
```

Verify:

```bash
swapon --show
```

---

## Layman Explanation

Emergency memory.

---

# 42. Nmap

Network scanner.

Scan host:

```bash
nmap 10.0.0.1
```

Scan ports:

```bash
nmap -p 22,80 host
```

OS detection:

```bash
sudo nmap -O host
```

Service scan:

```bash
nmap -sV host
```

---

## Layman Explanation

Security guard checking open doors.

---

