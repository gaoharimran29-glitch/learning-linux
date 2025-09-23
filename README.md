# 📑 Index

1. [Introduction to Linux](#-What-is-Linux?)
2. [SSH in linux)](#️-SSH)
3. [File System](#-Linux-File-System)

## What is Linux?

*Linux* is an open-source *operating system kernel* first created by *Linus Torvalds* in 1991.  
It powers servers, desktops, supercomputers, mobile devices (Android), IoT devices, and more.  

Unlike Windows or macOS, Linux is *free, open-source, secure, and highly customizable*.

---

##  Who Made Linux?

- *Creator:* Linus Torvalds (1991)  
- *License:* GNU General Public License (GPL)  
- *Community Driven:* Thousands of global contributors, companies, and foundations maintain Linux.

---

##  What is a Linux Distribution (Distro)?

A *Linux distribution (distro)* is a packaged version of Linux that includes:  
- The Linux kernel  
- System tools  
- Libraries  
- Applications  

### Popular Distros:
- *Ubuntu* → beginner friendly, backed by Canonical  
- *Debian* → very stable, parent of Ubuntu  
- *Fedora* → cutting-edge, backed by Red Hat  
- *Arch Linux* → lightweight, customizable  
- *CentOS / RHEL* → enterprise use  

👉 *Who Controls Linux?*  
- The *Linux Kernel* is overseen by Linus Torvalds + community.  
- *Distributions* are managed by different organizations (Ubuntu by Canonical, Fedora by Red Hat, etc.).

---

# SSH

---

## What is SSH?

*SSH (Secure Shell)* is a *cryptographic network protocol* that allows you to securely connect to another computer (often a server) over an insecure network.  

- Provides *encryption* (data is safe in transit)  
- Replaces insecure protocols like Telnet  
- Used for *remote login, file transfer, tunneling, and server management*

---

## How Can SSH Be Used?

1. *Remote Login:*
   
```bash
ssh username@server_ip
```

2. *File Transfer with SCP:*
   
```bash
scp file.txt user@server_ip:/path/to/destination
```

3. *File Transfer with SFTP (interactive):*

```bash
sftp user@server_ip
```

4. *Port Forwarding (Tunneling):*

```bash
ssh -L 8080:localhost:80 user@server_ip
```
---

## Common SSH Commands

| Command | Description |
|---------|-------------|
| ssh user@host | Connect to a remote server |
| ssh -p 2222 user@host | Connect using custom port |
| scp file.txt user@host:/path | Copy file to remote |
| scp user@host:/file.txt . | Copy file from remote |
| sftp user@host | Start SFTP session |
| ssh-keygen | Generate SSH keys |
| ssh-copy-id user@host | Copy key for passwordless login |

---

## Installing & Enabling SSH

### 1 Check if SSH is Installed

```bash
ssh -V
```

If not installed, install:

```bash

sudo apt update 
sudo apt install openssh-client openssh-server -y

```

### 2 Enable & Start SSH Service
```bash
sudo systemctl enable ssh 
sudo systemctl start ssh
```
Check service:

```bash
sudo systemctl status ssh
```
---

### 3️ Configure SSH (Optional but Recommended)

Config file path:
```bash
/etc/ssh/sshd_config
```
Examples:
```bash
Port 2222 PermitRootLogin no PasswordAuthentication yes
```
After changes:
```bash
sudo systemctl restart ssh
```
---

##  SSH Keys (for Passwordless Authentication)

Generate a key pair:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Copy public key to server:
```bash
ssh-copy-id user@server_ip
```
Now login without password:
```bash
ssh user@server_ip
```
---

## Security Best Practices for SSH

### Change default port (22 → something else)
- Edit the file and change port 22 to your custom port. Remove # before the port in file
- Suppose we are changing it to port 2222
  
```bash
sudo nano /etc/ssh/sshd_config
```
- Restart ssh
  
```bash
sudo systemctl restart ssh
```
- Now use this command from remote machine to acces your server.
  
```bash
sudo -p 2222 user@ip
```
### Disable root login

- Edit the file and change PermitRootLogin prohibit-password as PermitRootLogin no. Remove # before the port in file
 
```bash
sudo nano /etc/ssh/sshd_config
```
- Restart ssh
  
```bash
sudo systemctl restart ssh
```
- Now if you try below commands it will result an error
  
```bash
ssh root@ip
```
### Use SSH keys instead of passwords

- Edit the file and change PasswordAuthentication to no. Remove # before the port in file
 
```bash
sudo nano /etc/ssh/sshd_config
```
- Restart ssh
  
```bash
sudo systemctl restart ssh
```
- Now you can only use keys to login in server from remote
---

# Linux File System

## Philosophy
- **Everything is a file** → text files, directories, devices, processes, sockets.
- Consistency and simplicity across the OS.

---

## Filesystem Hierarchy Standard (FHS)

### Root `/`
Base of the filesystem tree. All directories start here.

### Key Directories
- `/bin` → Essential user binaries (ls, cp, mv).
- `/sbin` → System binaries (fdisk, iptables).
- `/etc` → System-wide config files.
- `/home` → User home directories.
- `/root` → Root user’s home.
- `/var` → Variable data (logs, cache).
- `/tmp` → Temporary files (cleared on reboot).
- `/usr` → User programs & data:
  - `/usr/bin` → Extra user commands
  - `/usr/sbin` → Extra system admin commands
  - `/usr/share` → Shared docs & man pages
- `/lib` → Shared libraries for `/bin` and `/sbin`.
- `/dev` → Device files (e.g. `/dev/sda`).
- `/proc` → Virtual FS (process/kernel info).
- `/sys` → Hardware/driver info.
- `/media` → Auto-mounted external devices.
- `/mnt` → Temporary mount points.
- `/opt` → Optional software packages.
- `/boot` → Boot loader files, kernel.

---

## File Types (`ls -l` first character)
- `-` → Regular file  
- `d` → Directory  
- `l` → Symbolic link  
- `c` → Character device  
- `b` → Block device  
- `p` → Named pipe  
- `s` → Socket  

---

## Permissions & Ownership
Format: `rwxr-xr--`
- 3 sets: owner, group, others.
- `r` = read, `w` = write, `x` = execute.

### Commands
- `chmod` → change permissions  
- `chown` → change owner  
- `umask` → default permissions for new files  

---

## Links
- **Hard link** → Same inode, another name.  
- **Soft link (symlink)** → Shortcut pointer to original.  

---

## Inodes
- Metadata of files (permissions, owner, timestamps, block locations).
- View with `ls -i`.

---

## Mounting & Partitions
- Devices must be **mounted** into FS tree.  
- `mount /dev/sdb1 /mnt`  
- `/etc/fstab` → persistent mounts.  

---

## Special Virtual FS
- `/proc` → Processes, system info (cpuinfo, meminfo).  
- `/sys` → Kernel and hardware configs.  
- `/dev` → Device files.  
- `tmpfs` → RAM-based temp FS.  

---

## Concepts
- **File Descriptors**:  
  - 0 = stdin  
  - 1 = stdout  
  - 2 = stderr  
- **Redirection**: `>`, `<`, `2>`, `|`  
- **Quotas** → Disk usage limits.  
- **Extended Attributes** → Extra metadata.  
- **ACLs** → Fine-grained permissions (`setfacl`).  

---

## Filesystem Types
- **ext4** → Default, stable.  
- **xfs** → High performance.  
- **btrfs** → Modern, snapshots, compression.  
- **zfs** → Advanced, enterprise-level.  
- **tmpfs** → RAM storage.  

---

## Commands
- Navigation: `pwd`, `cd`, `ls`  
- File ops: `cp`, `mv`, `rm`, `touch`  
- Info: `file`, `stat`, `ls -lh`  
- Search: `find`, `locate`, `grep`  
- Disk: `df -h`, `du -sh`, `lsblk`, `mount`, `umount`  

---

## Security Bits
- **SUID**: Run as owner (`s` bit).  
- **SGID**: Run as group.  
- **Sticky bit**: Only owner can delete in shared dirs (`/tmp`).  
- **SELinux/AppArmor**: Mandatory Access Control.  

---
