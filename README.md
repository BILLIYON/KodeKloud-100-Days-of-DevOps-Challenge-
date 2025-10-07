# 🚀 100 Days of DevOps Challenge  

This repository documents my journey through the **100 Days of DevOps Challenge (KodeKloud)**.  
Each day, I’ll log the task, solution, and key takeaways as I learn and practice DevOps concepts and tools.  

---

## 📅 Progress Log  

---

### ✅ Day 1 – Linux User Setup with Non-Interactive Shell  
**Task:**  
Create a user named `james` with a non-interactive shell on App Server 3 to accommodate a backup agent tool.  

**Solution:**  
```bash
sudo useradd -s /sbin/nologin james
grep james /etc/passwd
````

**Takeaway:**
Non-interactive shells like `/sbin/nologin` or `/bin/false` prevent direct logins but allow services/processes to run securely under the user.

---

### ✅ Day 2 – Linux Temporary User with Expiry
**Task:**  
Today’s challenge in the 100 Days of DevOps was about user lifecycle management. 
I created a developer account (siva) with a predefined expiry date to ensure temporary access for project needs.

**Solution:**  
```bash:
sudo useradd -e 2024-03-28 siva
sudo chage -l siva
````

**Takeaway:**
Setting expiry dates on user accounts is a best practice in DevOps and system administration. It helps enforce principle of least privilege, reduces risks from forgotten accounts, and ensures smooth offboarding for temporary team members.

---

### ✅ Day 3 – Disable Direct SSH Root Login
**Task:**  
Following security audits at xFusionCorp Industries, the security team required disabling direct SSH root login on all app servers in the Stratos Datacenter.

**Solution:**  
- SSH’d into each app server (`stapp01`, `stapp02`, `stapp03`).
- Edited `/etc/ssh/sshd_config`:
  - Set `PermitRootLogin no`.
- Restarted `sshd` service to apply changes.
- Verified that direct root login was disabled, while normal users retained sudo access

**Takeaway:**
Direct root SSH access is a critical security risk. By disabling it, we enforce the principle of least privilege—ensuring admins log in as regular users first, and only escalate when necessary.


---

### ✅ Day 4 – Script Execution Permissions
**Task:**  
A backup automation script (`xfusioncorp.sh`) had been distributed but wasn’t properly executable.

**Solution:**  
I used Linux file permissions (`chmod`) to ensure all users could run the script while keeping ownership intact.

**Key Commands:**
```bash
ls -l /tmp/xfusioncorp.sh   # check current permissions
chmod 755 /tmp/xfusioncorp.sh   # set proper executable rights
```
Permission after fix: -rwxr-xr-x 1

**Takeaway:**
Permissions are a critical part of Linux security.
Scripts often need to be readable and executable across users, but not always writable.
This was a small but essential step in automating and securing infrastructure.

---

### ✅ Day 5 - SElinux Installation and Configuration

**Task:**
Following a security audit, the xFusionCorp Industries security team decided to strengthen server security with **SELinux**. For App Server 1 in the Stratos Datacenter, I was asked to:

* Install the required SELinux packages.
* Permanently disable SELinux (until further configuration is ready).
* Ensure no reboot is performed immediately (changes should take effect after scheduled maintenance).

**Solution:**

1. **Install SELinux packages**

```bash
# CentOS/RHEL
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils

# Ubuntu/Debian
sudo apt update
sudo apt install -y selinux-basics selinux-policy-default auditd
```

2. **Edit SELinux config**

```bash
sudo vi /etc/selinux/config
```

Change:

```
SELINUX=enforcing
```

to:

```
SELINUX=disabled
```

3. **Verify changes**

```bash
grep SELINUX= /etc/selinux/config
# Output:
SELINUX=disabled
```

No reboot required now. After the scheduled reboot, SELinux will be permanently disabled. ✅

**Takeaway:**

* SELinux (Security-Enhanced Linux) enforces fine-grained access control, but sometimes needs to be temporarily disabled for system/application compatibility.
* Understanding how to configure SELinux is critical in system hardening and troubleshooting.
* Security is a balance: sometimes you prepare the environment first (packages installed) before enabling strict policies.

---

 ### ✅ Day 6 - Create a Cron Job

**Task:** Automating with Cron Jobs
On all Nautilus app servers in the Stratos Datacenter:
1. Install the **cronie** package (it provides the `crond` service for managing cron jobs).
2. Start the **crond** service so scheduled tasks can run.
3. Add a cron job for the **root user** that runs **every 5 minutes** and writes “hello” into `/tmp/cron_text`.

**Expected outcome:** Every 5 minutes, the file `/tmp/cron_text` will be overwritten with the word `hello`.

**Takeaway:**
In real-world DevOps, cron jobs are the backbone of automation:
* Scheduling database backups
* Rotating logs
* Syncing data between servers
* Running monitoring/health check scripts
Instead of relying on humans to remember, the system does the job *reliably* and *on time*.

---

### ✅ Day 7-  Linux SSH Authentication

**Task:**
xFusionCorp Industries required secure automation scripts to run on all application servers.
To make this possible, the `thor` user on the **jump host** needed **password-less SSH access** to all app servers via their respective sudo users (`tony`, `steve`, `banner`).


**Solution:**
1. **Generated SSH keys** for `thor` on the jump host:

   ```bash
   ssh-keygen -t rsa -b 4096
   ```

2. **Copied public keys** to each server’s sudo user:

   ```bash
   ssh-copy-id tony@stapp01
   ssh-copy-id steve@stapp02
   ssh-copy-id banner@stapp03
   ```

3. **Verified access** with simple logins:

   ```bash
   ssh tony@stapp01
   ```

 No password prompt, only key-based authentication.

4. (Optional) **Configured SSH aliases** in `~/.ssh/config` for easier server management.


**Takeaway**
Password-less SSH with key-based authentication is a **foundational DevOps skill**.
It:

* Enables **automation** (scripts, CI/CD, Ansible, etc.) without manual intervention.
* Enhances **security** (no password leaks).
* Improves **efficiency** in multi-server environments.

This small but critical setup is what makes large-scale operations **seamless and secure**.

---
## ✅ Day 8 - Installing Ansible for Automation

**Task:**
Set up Ansible on the Jump Host as the control node by installing version 4.10.0 with pip3, ensuring global accessibility.

**Steps Taken:**  
1. Installed `python3-pip` on the Jump Host.  

2. Installed Ansible 4.10.0 via pip3:

  ```bash
  sudo pip3 install ansible==4.10.0
  ```
Verified installation:
  ```
ansible --version
which ansible
  ```
Output confirmed Ansible is installed globally.

**Key Takeaway:**
Ansible’s simplicity (agentless, minimal setup) makes it ideal for quick configuration management and automation testing across servers.

---

## ✅ Day 9: MariaDB Troubleshooting

**Task**
The Nautilus production app couldn’t connect to the database. On investigation, the `mariadb` service was **down** on the DB server (`stdb01`).
The goal: **bring MariaDB back up and running.**

**Steps Taken**
1. **Tried restarting the service**

   ```bash
   sudo systemctl start mariadb
   ```

   → Failed with error.

2. **Checked logs**

   ```bash
   sudo systemctl status mariadb -l
   sudo journalctl -xeu mariadb
   ```

   Found errors like:

   ```
   InnoDB: Operating system error number 13
   InnoDB: The error means mysqld does not have the access rights to the directory.
   Cannot open datafile './ibtmp1'
   ```

3. **Diagnosed the issue**

   * Error 13 = **Permission Denied**.
   * Checked ownership of the data directory:

     ```bash
     ls -ld /var/lib/mysql
     ```

     Output showed it was owned by `root:mysql` (wrong).

4. **Fixed ownership and permissions**

   ```bash
   sudo chown -R mysql:mysql /var/lib/mysql
   sudo rm -f /var/lib/mysql/ibtmp1   # cleanup temp file
   ```

5. **Restarted MariaDB**

   ```bash
   sudo systemctl restart mariadb
   ```
Success → Service is active and running.

**Takeaway**

* Always check **logs first** (`systemctl status`, `journalctl`) — they tell the story.
* Error `13 (Permission Denied)` almost always means **ownership or SELinux issues**.
* MariaDB requires `/var/lib/mysql` and all contents to be owned by the `mysql` user.
* Real-world troubleshooting is about breaking the problem into **clues → diagnosis → fix**.

---
## ✅ Day 10 - Linux Bash Scripts

**Task:**  
Create a bash script (`/scripts/beta_backup.sh`) on App Server 3 that:
- Archives the `/var/www/html/beta` directory into `xfusioncorp_beta.zip`.
- Stores the archive in `/backup/`.
- Copies the archive to Nautilus Backup Server in `/backup/`.
- Works with passwordless SSH (no manual password prompt).
- Runs without sudo.

**Solution Steps:**  
1. Installed `zip` package.  
2. Created `/scripts` and `/backup` directories.  
3. Configured SSH key-based authentication from App Server 3 → Backup Server.  
4. Wrote and tested the script:

```bash
#!/bin/bash
SRC_DIR="/var/www/html/beta"
BACKUP_NAME="xfusioncorp_beta.zip"
LOCAL_BACKUP="/backup/$BACKUP_NAME"
REMOTE_USER="clint"
REMOTE_HOST="stbkp01"
REMOTE_DIR="/backup/"

zip -r $LOCAL_BACKUP $SRC_DIR
scp $LOCAL_BACKUP ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}
```
Made it executable:
```
chmod +x /scripts/beta_backup.sh
```

**Key Learning:**
Automating routine backup tasks with Bash + passwordless SSH reduces human error and ensures consistency. This forms the foundation for more advanced DevOps pipelines using Ansible, Jenkins, or cron jobs.

---

## ✅ Day 11 – Install and Configure Tomcat Server

**Task:**
The Nautilus Dev team built a Java-based beta application. My job was to:
Install Tomcat on App Server 2.
Configure it to run on port 6400.
Deploy the provided ROOT.war file so the app loads directly at the base URL (/).
Validate deployment with curl http://stapp02:6400.


**Solution:**

**Install Tomcat**
```
sudo yum install -y tomcat    # RHEL/CentOS
```

**Change Tomcat default port**
```
sudo vi /etc/tomcat/server.xml
```
**Update connector line:**
**<Connector port="6400" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />**

**Deploy WAR file**
```
scp /tmp/ROOT.war tony@stapp02:/tmp/
sudo mv /tmp/ROOT.war /var/lib/tomcat/webapps/
```

**Restart Tomcat**
sudo systemctl restart tomcat

**Test**
```
curl http://stapp02:6400
```


**Takeaway:**
This task reinforced:
How application servers like Tomcat host apps.
The importance of port reconfiguration for custom deployments.
Why naming the WAR file ROOT.war ensures deployment at the base URL without extra paths

---

## ✅ Day 12 - Linux Network Services

**Task:**
The monitoring tool reported that an Apache service was unreachable on port 8082 in Stratos Datacenter. My goal was to investigate why and restore accessibility from the Jump Host.

**Solution:**
Checked Apache service → Found it had failed to start.

Ran netstat -tulnp → Discovered Sendmail was already bound to port 8082 (PID 446).

✅ Killed the conflicting process.

✅ Restarted Apache successfully.

Verified locally with curl localhost:8082 → Worked fine.

Tested from Jump Host → Still unreachable.

Inspected firewall (iptables -L -n) → Default rules were rejecting all non-22 traffic.

✅ Added a rule to allow inbound traffic on port 8082.

✅ Confirmed remote access worked with curl http://stapp01:8082.


**Takeaway:**
Troubleshooting connectivity issues requires checking each layer systematically:
- Service status (systemctl status)
- Port binding conflicts (netstat -tulnp)
- Local connectivity (curl localhost)
- Firewall/network rules (iptables, firewalld, security groups).
This task was a great reminder that in real-world systems, problems are often multi-layered. Debugging is about patience, process, and persistence.

---

## ✅ Day 13 – IPtables Installation and Configuration

**Task:**
Today’s challenge focused on strengthening network security by configuring firewall rules using iptables across app servers.
*Scenario:*
Our web application runs on port 8083, but it was open to all inbound traffic, a potential security risk flagged by the team. The goal was to:

Install and configure iptables on all app servers.

Allow access to port 8083 only from the Load Balancer (LBR) host.

Ensure the rules persist after reboot.

**Solution:**

```bash
# Installed iptables and its services

sudo yum install -y iptables iptables-services

sudo systemctl enable iptables

sudo systemctl start iptables

# Configured rules
# Instead of simply appending rules (which might be overridden by existing REJECT rules), I inserted them at the top of the INPUT chain for proper priority:

sudo iptables -I INPUT 1 -p tcp --dport 8083 -s 172.16.238.14 -j ACCEPT

sudo iptables -I INPUT 2 -p tcp --dport 8083 -j DROP

Saved and verified configuration

sudo service iptables save

sudo iptables -L -n -v | grep 8083
```

**Takeaway:**
Rule order matters in iptables!

 If an ACCEPT or DROP rule sits below a general REJECT rule, it’ll never be executed. Always insert (-I) important rules before restrictive ones.

Ensuring persistence via service iptables save or /etc/sysconfig/iptables is critical to maintain security after a reboot.

---
## ✅ Day 14 – Linux Process Troubleshooting

**Task:**
The production support team of xFusionCorp Industries reported that Apache service was unavailable on one of the app servers.

My job was to:

Identify which app host had the issue.

Fix the Apache service so it runs on all app servers.

Ensure Apache listens on port 3003 across all servers.



**Solution:**

```bash
# Checked Apache status on each app server:

sudo systemctl status httpd

# Found that on one host, Apache failed to start with the error:

# (98)Address already in use: make_sock: could not bind to address [::]:3003

# Diagnosed the issue — port 3003 was already in use by another process.

# Used:

sudo netstat -tulnp | grep 3003

# Identified the conflicting process and killed it:

sudo kill -9 <PID>

# Restarted Apache and enabled it on boot:

sudo systemctl start httpd

sudo systemctl enable httpd

# Verified success:

sudo netstat -tulnp | grep httpd

curl http://localhost:3003

# Apache was now up and listening properly on port 3003 ✅

```

**Takeaway:**

- Troubleshooting service failures often comes down to reading the error logs carefully.

- In this case, the clue  “Address already in use”  pointed directly to the root cause.

- Always check for port conflicts before restarting or reinstalling services. Sometimes, the simplest fix is freeing up a busy port.


---

### ⏳ Day 15 – Setup SSL for Nginx

**Task:**
 Deploy and secure an Nginx web server with a self-signed SSL certificate on App Server 3.

 Steps included installing Nginx, moving provided certificate/key files, configuring SSL in Nginx, and verifying access over HTTPS.



**Solution:**

```bash

# 1. Switched to the root user to gain required privileges:

   ```bash
   sudo su -
   ```

# 2. Created the SSL directory and moved the certificate files:

   ```bash
   mkdir -p /etc/nginx/ssl
   mv /tmp/nautilus.crt /etc/nginx/ssl/
   mv /tmp/nautilus.key /etc/nginx/ssl/
   ```

# 3. Updated the Nginx configuration file `/etc/nginx/nginx.conf` with an SSL server block:

   ```nginx
   server {
       listen 443 ssl;
       server_name localhost;

       ssl_certificate /etc/nginx/ssl/nautilus.crt;
       ssl_certificate_key /etc/nginx/ssl/nautilus.key;

       root /usr/share/nginx/html;
       index index.html;
   }
   ```

# 4. Restarted and enabled Nginx:

   ```bash
   systemctl restart nginx
   systemctl enable nginx
   ```

# 5. Verified SSL setup using:

   ```bash
   curl -Ik https://localhost
   ```
```

**Takeaway:**
*Short reflection or lesson learned.*

---

### ⏳ Day 15 – \[Task Title Here]

**Task:**
*Description of the task goes here.*

**Solution:**

```bash
# commands or configuration used
```

**Takeaway:**
*Short reflection or lesson learned.*

---

## 🏁 Goals

* Stay consistent for 100 days.
* Gain hands-on experience across Linux, Docker, Kubernetes, CI/CD, Terraform, Ansible, Cloud, and more.
* Share progress daily on LinkedIn and GitHub.

---

## 📌 Resources

* [100 Days of DevOps Challenge – KodeKloud](https://kodekloud.com)
* [My LinkedIn](https://www.linkedin.com/in/john-abioye-4608001b9/)

---

## 📊 Progress Tracker

* [x] Day 1 – Linux User Setup with Non-Interactive Shell
* [x] Day 2 – Linux Temporary User with Expiry
* [x] Day 3 – Disable Direct SSH Root Login
* [x] Day 4 - Script Execution Permissions
* [x] Day 5 - SElinux Installation and Configuration
* [x] Day 6 - Create a Cron Job
* [x] Day 7 - Linux SSH Authentication
* [x] Day 8 - Installing Ansible for Automation
* [x] Day 9 - MariaDB Troubleshooting
* [x] Day 10 - Linux Bash Scripts
* [x] Day 11 - Install and Configure Tomcat Server
* [x] Day 13 - IPtables Installation and Configuration
* [x] Day 14 - Linux Process Troubleshooting
* [ ] Day 15 -
* [ ] Day 16 -
* [ ] Day 17 -
* [ ] Day 100 – 🎉

```



