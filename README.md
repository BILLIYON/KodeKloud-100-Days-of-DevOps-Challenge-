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

### ⏳ Day 9 – \[Task Title Here]

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
* [ ] Day 6 - Create a Cron Job
* [ ] Day 7 - Linux SSH Authentication
* [ ] Day 8 - Installing Ansible for Automation
* [ ] Day 9 -
* [ ] Day 10 -
* [ ] Day 100 – 🎉

```



