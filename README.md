
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

# SSH (Secure Shell)

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

## 🛠 Common SSH Commands

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

## 🛡 Security Best Practices for SSH

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
