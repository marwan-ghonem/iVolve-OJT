# Lab5: Ansible Playbook for Web Server Configuration

## 📌 Goal

Automate the setup of a web server using Ansible:
- Install Nginx
- Customize the default index page
- Verify the service

---

## 🖥️ Environment

- **Control Node**: Machine with Ansible installed
- **Managed Node**: Target machine for Nginx deployment

---

## 🧩 Step 1: Inventory File

Create a file named `inventory.ini`:
```ini
[webservers]
192.168.10.128 ansible_user=root
```

---

## 🛠️ Step 2: Create Ansible Playbook

Create a file named `webserver.yml`:
```yaml
---
- name: Configure Web Server
  hosts: webservers
  become: yes

  tasks:
    - name: Install Nginx
      yum:
        name: nginx
        state: present

    - name: Customize index.html
      copy:
        content: "<h1>Welcome to My Custom Nginx Page</h1>"
        dest: /usr/share/nginx/html/index.html

    - name: Start and enable Nginx
      service:
        name: nginx
        state: started
        enabled: true
```

---

## 🚀 Step 3: Run the Playbook

Run from the control node:
```bash
ansible-playbook -i inventory.ini webserver.yml
```

---

## ✅ Step 4: Verify on Managed Node

### SSH into the managed node:
```bash
ssh root@192.168.10.128
```

### Check Nginx:
```bash
systemctl status nginx
curl http://localhost
```

Expected output:
```
<h1>Welcome to My Custom Nginx Page</h1>
```

---

## 🧾 Notes

- Nginx must be allowed in the firewall if running on production.
- You can replace the `content` with a template or file for advanced use.
