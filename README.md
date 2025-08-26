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

### ⏳ Day 2 – \[Task Title Here]

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
* [ ] Day 2 – …
* [ ] Day 3 – …
* [ ] Day 100 – 🎉

```



