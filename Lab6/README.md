# Lab6: Ansible Roles for Application Deployment

## 📌 Goal

Automate the deployment of essential DevOps tools using Ansible roles:
- Docker
- Kubernetes CLI (`kubectl`)
- Jenkins (with Java 17)

---

## 🖥️ Environment

- **Control Node**: Machine running Ansible
- **Managed Node**: Target server to configure

---

## 🧩 Step 1: Inventory File

Create a file named `inventory.ini`:
```ini
[managed]
192.168.10.128 ansible_user=root
```

---

## 🛠️ Step 2: Ansible Playbook

Create a playbook file named `deploy_apps.yml`:
```yaml
---
- name: Deploy Applications
  hosts: managed
  become: yes

  roles:
    - docker
    - kubectl
    - jenkins
```

---

## 📦 Step 3: Role – Docker (`roles/docker/tasks/main.yml`)
```yaml
---
- name: Remove old versions of Docker if they exist
  yum:
    name: "{{ item }}"
    state: absent
  loop:
    - docker
    - docker-client
    - docker-client-latest
    - docker-common
    - docker-latest
    - docker-latest-logrotate
    - docker-logrotate
    - docker-engine

- name: Install required packages
  yum:
    name:
      - yum-utils
      - device-mapper-persistent-data
      - lvm2
    state: present

- name: Add Docker repo
  get_url:
    url: https://download.docker.com/linux/centos/docker-ce.repo
    dest: /etc/yum.repos.d/docker-ce.repo

- name: Install Docker CE
  yum:
    name:
      - docker-ce
      - docker-ce-cli
      - containerd.io
    state: present

- name: Start and enable Docker
  service:
    name: docker
    state: started
    enabled: true
```

---

## 📦 Step 4: Role – kubectl (`roles/kubectl/tasks/main.yml`)
```yaml
---
- name: Download kubectl
  get_url:
    url: https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kubectl
    dest: /usr/local/bin/kubectl
    mode: '0755'
```

---

## 📦 Step 5: Role – Jenkins (`roles/jenkins/tasks/main.yml`)
```yaml
---
- name: Install Java 17
  yum:
    name: java-17-openjdk
    state: present

- name: Add Jenkins repo
  yum_repository:
    name: jenkins
    description: Jenkins repo
    baseurl: http://pkg.jenkins.io/redhat-stable
    gpgcheck: no

- name: Install Jenkins
  yum:
    name: jenkins
    state: present

- name: Create Jenkins home if missing
  file:
    path: /var/lib/jenkins
    state: directory
    owner: jenkins
    group: jenkins
    mode: '0755'

- name: Start and enable Jenkins
  service:
    name: jenkins
    state: started
    enabled: true
```

---

## 🚀 Step 6: Run the Playbook

```bash
ansible-playbook -i inventory.ini deploy_apps.yml
```

---

## ✅ Step 7: Verify on Managed Node

```bash
docker --version
kubectl version --client
systemctl status jenkins
```

---

## 🧾 Notes

- Jenkins requires Java 17+
- You may need to disable SELinux temporarily: `setenforce 0`
- Jenkins runs on port 8080 by default
