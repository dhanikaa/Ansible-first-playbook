# **Ansible Beginner's Guide: Automating Configuration Management**  

## **Table of Contents**
- [Introduction](#introduction)  
- [Why Use Ansible?](#why-use-ansible)  
- [Ansible Architecture](#ansible-architecture)  
- [Installing Ansible on macOS](#installing-ansible-on-macos)  
- [Setting Up Passwordless SSH Authentication](#setting-up-passwordless-ssh-authentication)  
- [Managing Inventory with `inventory.ini`](#managing-inventory-with-inventoryini)  
- [Ad-hoc Commands vs. Playbooks](#ad-hoc-commands-vs-playbooks)  
- [Checking if Apache is Running](#checking-if-apache-is-running)  
- [Writing an Ansible Playbook](#writing-an-ansible-playbook)  
- [Running an Ansible Playbook](#running-an-ansible-playbook)  
- [Where to Find Ansible Modules](#where-to-find-ansible-modules)  
- [Conclusion](#conclusion)  

---

## **Introduction**  

**Ansible** is a powerful open-source **configuration management** and **automation tool** used to **manage infrastructure, automate deployments, and enforce security policies**. It is **agentless**, meaning it does not require software to be installed on the managed nodes.  

This guide covers everything a beginner needs to know about **Ansible**, including:  
✅ **Installation** and **Setup**  
✅ **Inventory Management**  
✅ **Ad-hoc Commands vs. Playbooks**  
✅ **Writing and Running Playbooks**  

---

## **Why Use Ansible?**  

Traditional methods like **Bash or Python scripts** require a lot of manual effort. Ansible simplifies infrastructure automation by providing:  

### ✅ **Advantages of Ansible**  
1. **Agentless** – No need to install an agent on managed nodes.  
2. **Idempotency** – Running a playbook multiple times won’t change the system if it’s already in the desired state.  
3. **Declarative** – Uses **YAML-based playbooks** that are easy to read and maintain.  
4. **Scalability** – Manages thousands of servers with a single command.  
5. **Built-in Modules** – Supports **package management, user management, file handling**, and more.  

---

## **Ansible Architecture**  

Ansible follows a **master-managed node** architecture:  

- **Master Node (Control Machine):**  
  - Runs Ansible and executes **playbooks**.  
  - Manages an **inventory file** to define target servers.  
- **Managed Nodes (Target Machines):**  
  - Servers where Ansible applies configurations.  
  - **Do not require Ansible installation**—only **SSH access** and **Python**.  

---

## **Installing Ansible on macOS**  

To install **Ansible** on macOS, use **Homebrew**:  

```bash
brew install ansible
```

Verify the installation with:  

```bash
ansible --version
```

---

## **Setting Up Passwordless SSH Authentication**  

Ansible requires **SSH access** to managed nodes. The best practice is to **use SSH key-based authentication**.  

### **Step 1: Generate an SSH Key**  
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible_key
```

### **Step 2: Copy the SSH Key to Managed Nodes**  
```bash
ssh-copy-id -i ~/.ssh/ansible_key.pub ubuntu@<server-ip>
```

Test the connection:  
```bash
ssh -i ~/.ssh/ansible_key ubuntu@<server-ip>
```

---

## **Managing Inventory with `inventory.ini`**  

Example `inventory.ini`:  

```ini
[web_servers]
server1 ansible_host=192.168.1.10 ansible_user=ubuntu
server2 ansible_host=192.168.1.11 ansible_user=ubuntu

[db_servers]
db1 ansible_host=192.168.1.20 ansible_user=ubuntu
```

---

## **Ad-hoc Commands vs. Playbooks**  

### **Ad-hoc Commands**  
Used for **quick one-time tasks** without writing a playbook.  

#### 🔹 **Ping all managed nodes**:  
```bash
ansible all -i inventory.ini -m ping
```

#### 🔹 **Ping a specific group (`web_servers`)**:  
```bash
ansible web_servers -i inventory.ini -m ping
```

#### 🔹 **Ping a specific node (`server1`)**:  
```bash
ansible server1 -i inventory.ini -m ping
```

---

## **Checking if Apache is Running**  

To check if **Apache** is running on all managed nodes:  

```bash
ansible all -i inventory.ini -m shell -a "systemctl status apache2"
```

To check it for a specific group (`web_servers`):  
```bash
ansible web_servers -i inventory.ini -m shell -a "systemctl status apache2"
```

To check it for a specific node (`server1`):  
```bash
ansible server1 -i inventory.ini -m shell -a "systemctl status apache2"
```

---

## **Writing an Ansible Playbook**  

### `first-playbook.yml`
```yaml
---
- hosts: all
  become: true
  tasks:
    - name: Install Apache HTTPD
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Copy index.html to web server
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html/index.html
        owner: root
        group: root
        mode: '0644'
```

---

## **Running an Ansible Playbook**  

```bash
ansible-playbook -i inventory.ini first-playbook.yml
```

---

## **Where to Find Ansible Modules?**  

🔗 **[Ansible Modules Documentation](https://docs.ansible.com/ansible/latest/collections/index.html)**  

---

## **Conclusion**  

Ansible simplifies **configuration management** and **automation**. With **inventory management, ad-hoc commands, and playbooks**, you can efficiently manage infrastructure.  

Now that you understand the basics, explore **Ansible roles, variables, and templates** for advanced automation! 🚀  
