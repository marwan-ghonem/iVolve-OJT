# Lab4: Ansible Setup for Remote Automation

## 📌 Goal

Automate remote task execution using Ansible:
- Configure SSH key-based access
- Create an inventory
- Run ad-hoc Ansible commands

---

## 🖥️ Machine Setup

- **Machine A**: Control Node (Ansible installed)
- **Machine B**: Managed Node (Remote host)

---

## 🪪 Step 1: Install Ansible on Machine A

### CentOS:
```bash
sudo dnf install epel-release -y
sudo dnf install ansible -y
```

### Verify:
```bash
ansible --version
```

---

## 🧩 Step 2: Create Inventory File

Create a file `inventory.ini`:
```ini
[managed]
192.168.10.128 ansible_user=root
```

---

## 🔐 Step 3: SSH Key Generation and Setup

### On Machine A:
```bash
ssh-keygen
```
(Press Enter to accept defaults)

### Copy the key to Machine B:
```bash
ssh-copy-id root@192.168.10.128
```

### Test connection:
```bash
ssh root@192.168.10.128
```

---

## 🧪 Step 4: Run Ansible Ad-Hoc Command

Run a test command:
```bash
ansible all -i inventory.ini -m shell -a "df -h"
```

Expected: Disk usage output from Machine B

---

## ✅ Final Check

If the command works:
- SSH is set up ✅
- Inventory is valid ✅
- Ansible is ready ✅

---

## 🧾 Notes

- Make sure firewalls are open between machines.
- SSH key must be under the user used in inventory.

