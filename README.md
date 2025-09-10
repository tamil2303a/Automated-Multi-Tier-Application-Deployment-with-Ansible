# 🚀 Ansible Multi-Tier Application Deployment

This project demonstrates the use of **Ansible** to automate the deployment of a **multi-tier web application** consisting of:

- **Frontend:** Nginx reverse proxy + static serving  
- **Backend:** Flask Python application  
- **Database:** MySQL  
- **Common Role:** System updates & dependencies  

It follows **Ansible best practices** with roles, templates, handlers, and environment-based inventories.

---

## 📂 Project Structure

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


---

## ⚙️ Features

- 🔹 Automated installation and configuration of **Nginx, Flask, and MySQL**
- 🔹 **Systemd service** for backend Flask app
- 🔹 **Reverse proxy** setup in Nginx to forward traffic to backend
- 🔹 **Database initialization** with user and schema creation
- 🔹 Use of **templates (Jinja2)** for dynamic configuration
- 🔹 **Handlers** for automatic service restarts on config changes
- 🔹 Environment separation with **staging** and **production** inventories

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

```bash
ansible-playbook -i inventory/staging.ini playbooks/site.yml
```

Verification

- Frontend (Nginx) → http://<frontend-server-ip>

- Backend (Flask API) → http://<backend-server-ip>:5000

- Database (MySQL) → Accessible at port 3306
