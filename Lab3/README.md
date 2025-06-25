
---

## 📁 `Lab-3_SSH_Config/README.md`

```markdown
# 🔐 Lab 3: SSH Key Authentication and Alias Setup

## 🎯 Objective

Improve SSH security and convenience by:
- Using SSH key-based authentication
- Creating an alias to connect with a simple `ssh ivolve` command

---

## 🛠️ Environment

- Machine A (Local): `192.168.10.138`
- Machine B (Remote): `192.168.10.128`
- User: `root`

---

## 📋 Implementation Steps

### 1️⃣ Generate SSH Key on Machine A
```bash
ssh-keygen -t rsa -b 4096 -C "marwan@machineA"

2️⃣ Copy Public Key to Machine B

ssh-copy-id root@192.168.10.128

3️⃣ Configure SSH Alias
Edit (on Machine A):

nano ~/.ssh/config

Host ivolve
  HostName 192.168.10.128
  User root
  IdentityFile ~/.ssh/id_rsa
