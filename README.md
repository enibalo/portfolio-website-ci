# Portfolio Website CI/CD Pipeline

## 1. Project Overview

This independent project demonstrates a fully automated CI/CD pipeline for deploying a static portfolio website using modern DevOps tools and practices. The pipeline continuously monitors a GitHub repository for changes, automatically tests the website code, and deploys validated updates to a production Linux web server.

The solution uses Jenkins hosted on an AWS EC2 Ubuntu instance as the automation server. GitHub webhooks notify Jenkins whenever new code is pushed to the repository. Jenkins then executes automated Python unit tests and HTML validation checks using BeautifulSoup before securely deploying the updated website to the production server using SCP.

### Technologies Used

* Git & GitHub
* Jenkins
* AWS EC2
* Ubuntu Linux
* Python - BeautifulSoup
* SCP / SSH
* Linux Web Server
* HTML & CSS

### Key Features

* Automated CI/CD deployment pipeline
* GitHub webhook integration
* Automated HTML validation testing
* Secure SCP deployment
* Jenkins email notifications
* AWS-hosted automation server
* Production deployment to Linux server

---

# 2. Architecture Diagram

```text
+-------------------+
|     Developer     |
+-------------------+
          |
          | Push Code
          v
+-------------------+
|      GitHub       |
+-------------------+
          |
          | Webhook Trigger
          v
+-----------------------------+
|    AWS EC2 Ubuntu Server  
  |
|        - Unit tests         |
+-----------------------------+
          |
          | Deployment
          v
+-----------------------------+
|       Linux Web Server      |
+-----------------------------+
```

---

# 3. Work Flow Diagram

```text
+-------------------+
| Developer Updates |
| Portfolio Website |
+-------------------+
          |
          | git push
          v
+-------------------+
|      GitHub       |
| Repository Update |
+-------------------+
          |
          | Webhook Trigger
          v
+----------------------------+
| Jenkins Pipeline Starts    |
| on AWS EC2 Ubuntu Server   |
+----------------------------+
          |
          | Clone Repository
          v
+----------------------------+
| Jenkins Executes Python    |
| Unit Tests + HTML Checks   |
+----------------------------+
          |
     +----+-----------------------------+
     |                                  |
Tests Pass                          Tests Fail
     |                                  |
     v                                  v
+----------------------+     +----------------------+
| SCP Deploy Website   |     | Send Failure Email   |
| to School Linux Host |     | Notification         |
+----------------------+     +----------------------+
          |
          | Update ~/www directory
          v
+----------------------------+
| Production Website Updated |
+----------------------------+
          |
          | Success Email
          v
+----------------------------+
| Deployment Notification    |
| Sent to Developer          |
+----------------------------+
```


# 4. My Accomplisments 

**Resolving a Criticial Pipeline Failure**

While setting up automated deployments from Jenkins to my web server using the SSH Agent plugin, the deployment kept failing whenever Jenkins tried to use the stored SSH credentials. To troubleshoot the issue, I reviewed the error logs, checked the plugin documentation, and explored the Jenkins file structure to understand how the plugin was creating temporary SSH files.

I discovered that the problem was caused by spaces in my Jenkins pipeline name. The plugin was generating file paths incorrectly because it could not properly handle the spaces, causing it to look for files that did not exist. I fixed the issue by renaming the project to remove spaces from the pipeline name, which restored successful automated deployments.

**Halving Time For Documentation By 50%**

Looked into the advance techqnies of using AI. Utilized AI to create first-drafts for documentation and README. Creating effective documentaiton 

Used advance technqiues such as: 
- Asking AI to criticize my prompts 
- Using README.md notation which AI understands better 


**Ensuring Project Longevity and Stability**

Followed indusrry-best practices .
Created architecture diagrams and workflow diagrams allow other engineers to trace connections and dependencies without digging through config files. 

Useful for debugging and collaboration. 