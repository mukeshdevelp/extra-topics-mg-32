# Second Week Topics

## Table of Contents
- [Package Management](#package-management)
- [SSH Command](#ssh-command)
- [Sed and Grep Command](#sed-and-grep-command)
- [AWK Command](#awk-command)
- [LVM in Linux](#lvm-in-linux)
- [Linux Networking](#linux-networking)

## Package Management
Package management is the process of installing, updating, upgrading, and removing software on a Linux system.

### Key Points
- Helps manage software dependencies.
- Keeps packages updated and secure.
- Common tools vary by Linux distribution.

### Common Commands
- `apt` for Debian/Ubuntu-based systems.
- `yum` or `dnf` for RHEL-based systems.
- `rpm` for low-level package handling.

### Example
```bash
sudo apt update
sudo apt install vim
```

## SSH Command
SSH (Secure Shell) is used to securely connect to remote Linux systems over a network.

### Key Points
- Provides encrypted remote access.
- Commonly used for server administration.
- Supports key-based authentication.

### Common Commands
- `ssh user@host`
- `ssh -i key.pem user@host`
- `scp` for secure file transfer.

### Example
```bash
ssh ubuntu@192.168.1.10
```

## Sed and Grep Command
`sed` is used for stream editing, while `grep` is used for searching text patterns in files and command output.

### Key Points
- `grep` finds matching text.
- `sed` modifies text streams.
- Both are widely used in shell scripting.

### Common Commands
- `grep "pattern" file.txt`
- `grep -i "pattern" file.txt`
- `sed 's/old/new/g' file.txt`

### Example
```bash
grep "error" app.log
sed 's/linux/LINUX/g' file.txt
```

## AWK Command
AWK is a powerful text-processing tool used for pattern matching, report generation, and data extraction.

### Key Points
- Works well with structured text.
- Can process columns and fields.
- Useful for log analysis and reporting.

### Common Commands
- `awk '{print $1}' file.txt`
- `awk -F',' '{print $2}' file.csv`

### Example
```bash
awk '{print $1, $3}' data.txt
```

## LVM in Linux
LVM (Logical Volume Manager) provides flexible disk management in Linux.

### Key Points
- Allows resizing logical volumes.
- Makes storage management easier.
- Supports snapshots and better space allocation.

### Components
- Physical Volume (PV)
- Volume Group (VG)
- Logical Volume (LV)

### Example
```bash
pvcreate /dev/sdb
vgcreate myvg /dev/sdb
lvcreate -n mylv -L 10G myvg
```

## Linux Networking
Linux networking covers configuration and troubleshooting of network interfaces, routes, DNS, and connectivity.

### Key Points
- Used to configure and debug network settings.
- Includes IP, routing, DNS, and firewall basics.
- Essential for servers and cloud environments.

### Common Commands
- `ip a`
- `ip r`
- `ping`
- `netstat` or `ss`
- `curl`

### Example
```bash
ip a
ping 8.8.8.8
ss -tuln
```
