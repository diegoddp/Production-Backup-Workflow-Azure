# 🛡️ Production Backup Workflow

This repository provides a secure and automated workflow to back up both the site content (App Service `wwwroot`) and the MySQL database from a **production Azure App Service**. The backup is initiated via GitHub Actions, which triggers an Azure DevOps pipeline to perform the actual backup and push the contents into this GitHub repository.

---

## 🔁 Workflow Overview

### 1. GitHub Actions – Trigger the Backup

The `Backup From Production` GitHub Actions workflow allows developers to trigger a backup request manually using the **workflow_dispatch** event.

When triggered, it:

- Initiates the Azure DevOps pipeline (`Backup-to-Github`)
- Monitors the pipeline execution
- Fails the GitHub Action if the Azure pipeline fails

> 📍 Trigger this manually from GitHub via the Actions tab.

### 2. Azure DevOps – Perform the Backup

The Azure DevOps pipeline performs the following tasks:

- Downloads the App Service `wwwroot` folder using the **Kudu API**
- Creates a **MySQL database backup** using `mysqldump`
- Packages both backups
- Pushes them into a `remediations_dump` branch in this repository

---
### Azure DevOps Pipeline Summary
Pipeline Name: Backup-to-Github

Key Actions:

Download wwwroot via Kudu API

Dump MySQL database via mysqldump

Store files into structured folders:

artifact/wordpress/

artifact/db/

Initialize Git repo and push to branch remediations_dump in this GitHub repo

Sensitive inputs like credentials, MySQL access, and GitHub PAT are pulled from Azure DevOps variable groups and GitHub secrets.

---

🔐 Security Considerations
Azure DevOps pipeline uses service principal authentication and variable groups.

GitHub Action uses a Personal Access Token (PAT) stored securely in secrets.AZURE_DEVOPS_PAT.

Database credentials and Kudu credentials are never stored in code – all pulled securely at runtime.
