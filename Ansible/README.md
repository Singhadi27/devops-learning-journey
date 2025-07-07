# 🔧 DevOps with Ansible – Learning Journey (Class 21–25)

This repository documents my hands-on journey learning **Ansible** — a powerful automation engine used for configuration management, application deployment, and infrastructure orchestration.

---

## 📌 What is Ansible?

- ✅ Free & Open Source
- ⚙️ Used for automating server creation to application deployment
- 🛠 Known as a **Configuration Management Tool**
- 🧠 Written in Python and uses **YAML** for configuration files
- 🖥 GUI tool: **Ansible Tower**

---

## 📘 Key Concepts

| Term       | Description                                                                 |
|------------|-----------------------------------------------------------------------------|
| **Playbook** | YAML file containing the automation instructions                          |
| **Inventory**| File that defines the target hosts or groups                              |
| **Modules**  | Building blocks used in Playbooks (e.g., yum, apt, copy, service)         |
| **Tasks**    | Individual actions within a playbook                                      |
| **Tags**     | Used to run/skip specific tasks                                           |
| **Handlers** | Triggered after a change (e.g., restart service after config change)      |
| **Facts**    | Auto-collected data about hosts (CPU, RAM, OS, etc.)                      |
| **Vault**    | Encrypts sensitive data like passwords                                    |

---

## ⚙️ Initial Setup

1. **Install Ansible** (on controller node):
   ```bash
   sudo yum install epel-release -y
   sudo yum install ansible -y
   ```

2. **Set Hostnames Individually** on each server.

3. **Enable Root Access**:
   ```bash
   sudo -i
   passwd root
   ```

4. **Define Worker IPs** in Ansible Inventory:
   ```bash
   vim /etc/ansible/hosts
   ```

5. **Generate SSH Key & Share to Workers**:
   ```bash
   ssh-keygen   # Press Enter 4 times
   ssh-copy-id root@<worker-ip>
   ```

6. **Test Ansible is Working**:
   ```bash
   ansible all -m ping
   ```

---

## 📦 Playbook Example

```yaml
---
- name: Install Git on all nodes
  hosts: all
  become: yes
  tasks:
    - name: Install git
      yum:
        name: git
        state: present
```

Run it with:
```bash
ansible-playbook install-git.yml
```

---

## 🏷 Using Tags

**Assigning & Executing Tags**:
```yaml
tasks:
  - name: Install Apache
    yum:
      name: httpd
      state: present
    tags: apache

  - name: Start Apache
    service:
      name: httpd
      state: started
    tags: start
```

Run specific tags:
```bash
ansible-playbook playbook.yml --tags "apache,start"
```

Skip specific tags:
```bash
ansible-playbook playbook.yml --skip-tags "debug"
```

---

## 🔁 Shell vs Command vs Raw Module

| Module   | Use Case              |
|----------|-----------------------|
| `raw`    | Executes raw shell cmd (no JSON parsing) |
| `command`| Secure, recommended   |
| `shell`  | Used for complex bash logic |

---

## 🔁 Replacing Text in Playbook

Change "install" to "remove" in a file:
```bash
sed -i 's/install/remove/g' playbook.yml
```

Replace variables in Vim:
```
:%s/absent/present/g
```

---

## 🔁 Loops & Conditions

Loops help run tasks multiple times:
```yaml
with_items:
  - git
  - httpd
  - vim
```

Use `when` to run conditionally:
```yaml
tasks:
  - name: Install using yum on RedHat
    yum:
      name: httpd
      state: present
    when: ansible_os_family == "RedHat"

  - name: Install using apt on Ubuntu
    apt:
      name: apache2
      state: present
    when: ansible_os_family == "Debian"
```

---

## 🧪 Troubleshooting

```bash
ansible all -m setup | grep -i cpus
ansible all -m setup | grep -i mem
```

---

## 🔐 Ansible Vault

Used to encrypt sensitive files:
```bash
ansible-vault create secrets.yml
ansible-vault edit secrets.yml
ansible-vault decrypt secrets.yml
```

Use vault in playbook:
```yaml
vars_files:
  - secrets.yml
```

---

## 🌐 LAMP Stack Deployment

> LAMP = Linux + Apache + MySQL + PHP

1. Get playbook: `netflix.yml` from `all-setup` repo  
2. Run the playbook:
   ```bash
   ansible-playbook netflix.yml
   ```
3. Visit the IP in browser to confirm Netflix clone is running

---

## ☕ Tomcat Java App Deployment

1. Create `tomcat.yml`
2. Paste code from `all-setup` repo
3. Run:
   ```bash
   ansible-playbook tomcat.yml
   ```
4. Tomcat will be installed and deployed

---

## ✅ Best Practices & Notes

- Always define inventory in `/etc/ansible/hosts`
- Never disable `gather_facts` unless required
- Use tags for selective execution
- Encrypt sensitive values with Vault
- Reuse roles/playbooks across environments (DEV, QA, PROD)
- Use conditionals for heterogeneous server environments

---

## 📚 Summary

| Class | Topic                             |
|-------|-----------------------------------|
| 21    | Introduction to Ansible, Setup    |
| 22    | Playbooks, Modules, Tags          |
| 23    | Shell vs Command vs Raw, Variables|
| 24    | Loops, Conditions, Vault          |
| 25    | LAMP Stack & Tomcat Deployment    |

---

## 🙌 Conclusion

With Ansible, you can automate everything from app deployment to environment provisioning with just a few lines of YAML. This journey helped me understand the power of infrastructure-as-code in real-world DevOps.

🔗 Feel free to fork the repo, run the playbooks, and drop a ⭐ if you learned something!
