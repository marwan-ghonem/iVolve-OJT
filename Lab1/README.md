# 🔐 Lab 1: MySQL Backup Automation Using Shell Script and Cron

## 🎯 Objective

Automate the process of backing up all MySQL databases daily by:
- Writing a Bash script that performs the backup
- Scheduling the script to run automatically using `cron`
- Ensuring backups are safely stored in a dedicated directory

---

## 🛠️ Environment
- OS: CentOS Stream 9
- Tools: MySQL Server, Shell scripting, Cron

---

## 📋 Implementation Steps

### 1️⃣ Install MySQL Server
```bash
sudo dnf install -y https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo dnf install -y mysql-server
sudo systemctl start mysqld
sudo systemctl enable mysqld

2️⃣ Secure MySQL and Set Root Password

sudo mysql_secure_installation

3️⃣ Create Backup Directory and Script
Script: mysql_backup.sh


#!/bin/bash
BACKUP_DIR="$HOME/mysql_backups"
DATE=$(date +%F)
FILENAME="$BACKUP_DIR/mysql_backup_$DATE.sql"
mkdir -p "$BACKUP_DIR"
mysqldump --all-databases > "$FILENAME"
4️⃣ Make Script Executable

chmod +x mysql_backup.sh
5️⃣ Schedule Daily Backup Using Cron

crontab -e
