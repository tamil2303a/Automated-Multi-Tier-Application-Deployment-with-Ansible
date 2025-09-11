# 🚀 Ansible Multi-Tier Application Deployment

This project demonstrates the use of Ansible to automate the deployment of a multi-tier application consisting of a frontend (Nginx), a backend API (Flask app), and a database (MySQL). It is designed to reflect a real-world infrastructure setup where applications are deployed across multiple servers, ensuring scalability, maintainability, and automation.

## 📖 Project Overview

In modern DevOps practices, manual provisioning of servers and configuring services is inefficient and error-prone. This project showcases how Infrastructure as Code (IaC) principles can be applied using Ansible to automate:

- Installing and configuring system dependencies

- Deploying a backend Python Flask application as a service

- Setting up and securing a MySQL database

- Deploying an Nginx reverse proxy as the frontend to route requests to the backend

- Managing configurations through templates (Jinja2) and handlers to ensure smooth service restarts

The solution is structured using Ansible roles and environment-specific inventories, making it modular, reusable, and production-ready.

---

## 📂 Project Structure

```bash
Automated-Multi-Tier-Application-Deployment-with-Ansible/
├── README.md
├── ansible.cfg
├── inventory/
│ ├── staging.ini
│ └── production.ini
├── playbooks/
│ └── site.yml
├── roles/
│ ├── common/
│ │ ├── tasks/main.yml
│ │ └── handlers/main.yml
│ ├── backend/
│ │ ├── tasks/main.yml
│ │ ├── templates/flask.service.j2
│ │ ├── handlers/main.yml
│ │ └── files/app.py
│ ├── frontend/
│ │ ├── tasks/main.yml
│ │ └── templates/nginx.conf.j2
│ └── database/
│ ├── tasks/main.yml
│ └── templates/db_setup.sql.j2
```
Each role is isolated and responsible for one tier of the architecture. This separation of concerns follows best practices in configuration management.

## ⚙️ Steps Involved

1. Inventory Setup
Define environment-specific inventory files ([staging.ini](staging.ini) and [production.ini](production.ini)). Each file contains groups of servers categorized as frontend, backend, and database.

2. Playbook Execution
The main playbook [site.yml](site.yml) orchestrates the provisioning. It applies roles in sequence:

  - common → install base dependencies
  
  - database → set up MySQL and users
  
  - backend → deploy Flask app as a systemd service
  
  - frontend → configure Nginx as a reverse proxy

3. Backend Application Deployment
The Flask backend is copied to the server and managed via systemd, ensuring it runs continuously and restarts on failures or config changes.

4. Frontend Reverse Proxy Setup
Nginx is configured through Jinja2 templates to forward requests to the backend service, ensuring loose coupling between tiers.

5. Database Configuration
MySQL is installed, a new database is created, and users are provisioned with appropriate privileges. Passwords and sensitive data can be secured using Ansible Vault for production.

6. Handlers & Idempotency
Handlers restart services (like Flask or Nginx) only when configuration files change, preventing unnecessary downtime. The playbooks are idempotent, meaning they can be re-applied safely without causing duplicate configurations.

---

## ⚙️ Features


- Automated installation and configuration of **Nginx, Flask, and MySQL**
- **Systemd service** for backend Flask app
- **Reverse proxy** setup in Nginx to forward traffic to backend
- **Database initialization** with user and schema creation
- Use of **templates (Jinja2)** for dynamic configuration
- **Handlers** for automatic service restarts on config changes
- Environment separation with **staging** and **production** inventories

---

## 🛠️ Setup Instructions

### 1️ Clone the repository
```bash
git clone https://github.com/your-username/ansible-multitier-app.git
cd ansible-multitier-app
```
### 1️ Configure Inventory

Edit [inventory/staging.ini](staging.ini) with your server details:
```ini
[frontend]
staging-frontend ansible_host=192.168.1.10 ansible_user=ubuntu

[backend]
staging-backend ansible_host=192.168.1.11 ansible_user=ubuntu

[database]
staging-db ansible_host=192.168.1.12 ansible_user=ubuntu
```

### 3 Run the Playbook

Run the playbook against the staging environment:
```bash
ansible-playbook -i inventory/staging.ini playbooks/site.yml
```

Or against production:
```bash
ansible-playbook -i inventory/production.ini playbooks/site.yml
```

## 🔍 Verification

- Frontend (Nginx) → http://<frontend-server-ip>

- Backend (Flask API) → http://<backend-server-ip>:5000

- Database (MySQL) → Accessible at port 3306
