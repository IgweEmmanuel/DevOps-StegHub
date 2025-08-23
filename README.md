# Installing Git and OpenSSH Server on WSL (Windows Subsystem for Linux)

This guide covers installing Git and OpenSSH Server on Windows Subsystem for Linux (WSL).

## Prerequisites

- Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11
- WSL installed and configured
- Ubuntu or your preferred Linux distribution running on WSL

## Installing Git on WSL

### Update Package Lists

First, ensure your package lists are up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

### install Git

```bash
sudo apt install git -y
```

### Verify Git installation

```bash
git --version
```

### Configure Git for first time setup

---

### Set your name and email

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Optional: Set default branch name

```bash
git config --global init.defaultBranch main
```

### Optional: Set default editor

```bash
git config --global core.editor "code --wait"  # For VS Code
# or
git config --global core.editor "nano"         # For nano
```

### Verify configuration

```bash
git config --list
```

## Installing OpenSSH Server on WSL

### Installing OpenSSH Server

```bash
sudo apt install openssh-server -y
```

### Configure SSH Server

```bash
sudo nano /etc/ssh/sshd_config
```

### Add or modify these lines in /etc/ssh/sshd_config:

```bash
# Change default port (optional, for security)
Port 2222

# Allow password authentication (can disable later after setting up keys)
PasswordAuthentication yes

# Allow public key authentication
PubkeyAuthentication yes

# Disable root login (recommended)
PermitRootLogin no

# Allow your user (replace 'username' with your actual username)
AllowUsers your_wsl_username
```

### Start and enable SSH Service

```bash
# Start SSH service
sudo service ssh start

# Check SSH service status
sudo service ssh status

# Optional: Auto-start SSH on WSL startup
echo 'sudo service ssh start' >> ~/.bashrc
```
