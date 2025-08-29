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

### ⏳ Day 5 – \[Task Title Here]

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
* [ ] Day 5 -
* [ ] Day 6 -
* [ ] Day 100 – 🎉

```



