 
## Configuration Management Using Ansible

---

## 📌 Project Overview

This project demonstrates **Configuration Management using Ansible**.  
The goal is to automate the installation and configuration of a web server (Nginx) using:

- Inventory
- Playbooks
- Variables
- Roles
- Handlers
- Idempotency


Tools Used

- AWS EC2 (Ubuntu Free Tier)
- Ansible
- Git & GitHub


Project Structure

ansible-task/
│
├── inventory
├── site.yml
├── output.log
├── README.md
└── nginx-role/
├── tasks/
│ └── main.yml
├── handlers/
├── vars/
├── defaults/
└── meta/


Step 1: Launch AWS Ubuntu Server

1. Create EC2 Instance (Ubuntu)
2. Allow port:
   - 22 (SSH)
   - 80 (HTTP)
3. Connect using: ssh -i your-key.pem ubuntu@your-public-ip

 Step 2: Install Ansible

Update system:
sudo apt update
sudo apt upgrade -y


Install Ansible: sudo apt install ansible -y

Check version: ansible --version

Step 3: Configure Inventory

Create inventory file:
nano inventory


Add: [local]
localhost ansible_connection=local


Test inventory:  ansible -i inventory local -m ping


Expected Output: pong


 Step 4: Create Role

Generate role:  ansible-galaxy init nginx-role

Edit task file:  nano nginx-role/tasks/main.yml

Add:
---
- name: Install nginx
  apt:
    name: nginx
    state: present

Step 5: Create Main Playbook

Create: nano site.yml


Add:
---
- hosts: local
  become: yes
  roles:
    - nginx-role

Step 6: Run Playbook
ansible-playbook -i inventory site.yml

Step 7: Verify Installation

Check service: systemctl status nginx


Open browser: http://your-public-ip

Nginx default page should appear.

Idempotency Check
Run playbook again:  ansible-playbook -i inventory site.yml

If output shows:
changed=0

It means playbook is idempotent.

Save Execution Logs
ansible-playbook -i inventory site.yml > output.log
