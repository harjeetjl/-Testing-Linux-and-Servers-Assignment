# 📜 DevOps Assignment Report  
## Setting Up and Managing a Secure Development Environment  

**Submitted by:** Harjeet J L 

---  

## 1. Introduction  
This report documents the setup and configuration of a secure, monitored, and well-maintained development environment for two new developers, **Sarah and Mike**. The project involved:  
- **System Monitoring Setup** 📊  
- **User Management and Access Control** 🔐  
- **Backup Configuration for Web Servers** 💾  

Each section includes implementation details, screenshots, and logs as proof of successful execution.  

---  

## 2. Implementation Steps  

### 📌 Task 1: System Monitoring Setup  
#### **Objectives:**  
- Install and configure **htop** for CPU, memory, and process monitoring  
- Set up **disk usage tracking** using `df` and `du`  
- Identify and log **resource-intensive processes**  
- Automate system monitoring using **cron jobs**  

#### **Implementation:**  
✅ Installed **htop** and verified it using:  
```bash
sudo apt update && sudo apt install -y htop 
```  
✅ Configured **disk usage monitoring** and logged the output:  
```bash
df -h > ~/system_monitor.log
du -ah /var | sort -rh | head -10 >> ~/system_monitor.log
```  
✅ Logged **top CPU and memory-consuming processes**:  
```bash
ps aux --sort=-%cpu | head -10 >> ~/system_monitor.log
ps aux --sort=-%mem | head -10 >> ~/system_monitor.log
```  
✅ Automated monitoring using a **cron job** (`system_monitor.sh`) running **every hour**.  

---  

### 📌 Task 2: User Management & Access Control  
#### **Objectives:**  
- Create user accounts for **Sarah** and **Mike**  
- Set up **isolated workspaces**  
- Enforce a **password policy**  

#### **Implementation:**  
✅ Created user accounts:  
```bash
sudo useradd -m -s /bin/bash Sarah
sudo useradd -m -s /bin/bash Mike
```  
✅ Set secure passwords:  
```bash
sudo passwd Sarah
sudo passwd Mike
```  
✅ Created **workspace directories**:  
```bash
sudo mkdir -p /home/Sarah/workspace
sudo mkdir -p /home/Mike/workspace
```  
✅ Secured access using **permissions**:  
```bash
sudo chown Sarah:Sarah /home/Sarah/workspace
sudo chmod 700 /home/Sarah/workspace
sudo chown Mike:Mike /home/Mike/workspace
sudo chmod 700 /home/Mike/workspace
```  
✅ Enforced **password expiration policy** (30 days):  
```bash
sudo chage -M 30 Sarah
sudo chage -M 30 Mike
```  
---  

### 📌 Task 3: Backup Configuration for Web Servers  
#### **Objectives:**  
- Automate backups for **Sarah's Apache server** and **Mike's Nginx server**  
- Store backups in `/backups/` with proper naming  
- Schedule backups using **cron jobs** (Every Tuesday at 12:00 AM)  
- Verify backup integrity  

#### **Implementation:**  
✅ Created a **backup directory**:  
```bash
sudo mkdir -p /backups
sudo chmod 777 /backups
```  
✅ Configured **Apache backup script** (`apache_backup.sh`):  
```bash
#!/bin/bash
BACKUP_DIR="/backups"
TIMESTAMP=$(date +%F)
BACKUP_FILE="$BACKUP_DIR/apache_backup_$TIMESTAMP.tar.gz"

tar -czf $BACKUP_FILE /etc/apache2/ /var/www/html/
echo "Apache backup created: $BACKUP_FILE" >> /var/log/backup.log
```  
✅ Configured **Nginx backup script** (`nginx_backup.sh`):  
```bash
#!/bin/bash
BACKUP_DIR="/backups"
TIMESTAMP=$(date +%F)
BACKUP_FILE="$BACKUP_DIR/nginx_backup_$TIMESTAMP.tar.gz"

tar -czf $BACKUP_FILE /etc/nginx/ /usr/share/nginx/html/
echo "Nginx backup created: $BACKUP_FILE" >> /var/log/backup.log
```  
✅ Made the scripts executable:  
```bash
sudo chmod +x /usr/local/bin/apache_backup.sh
sudo chmod +x /usr/local/bin/nginx_backup.sh
```  
✅ Scheduled **cron jobs** to run **every Tuesday at 12:00 AM**:  
```bash
sudo crontab -e
```  
Added:  
```
0 0 * * 2 /usr/local/bin/apache_backup.sh
0 0 * * 2 /usr/local/bin/nginx_backup.sh
```  
✅ Verified backup integrity:  
```bash
ls -lh /backups/
tar -tzf /backups/apache_backup_$(date +%F).tar.gz
tar -tzf /backups/nginx_backup_$(date +%F).tar.gz
```    

## 3. Challenges & Solutions  
| **Challenge** | **Solution** |  
|--------------|------------|  
| Apache config directory not found (`/etc/httpd/` missing) | Changed to `/etc/apache2/` after checking with `apachectl -V` |  
| Permission denied for logging system monitoring | Used `sudo` and adjusted script to allow logging |  

---  

## 4. Conclusion  
This project successfully implemented **system monitoring, secure user management, and automated backups**.
By automating these processes, we ensured:  
✔️ **Efficient monitoring of system resources**  
✔️ **Secure and isolated access control**  
✔️ **Automated backups for disaster recovery**  


---  

## 5.Screenshots & Logs  
**Attached all screenshots/logs.**  

