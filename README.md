# Portfolio Website CI/CD Pipeline

## 1. Project Overview

This independent project demonstrates a fully automated CI/CD pipeline for deploying a static portfolio website using modern DevOps tools and practices. The pipeline continuously monitors a GitHub repository for changes, automatically tests the website code, and deploys validated updates to a production Linux web server.

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
* Jenkins email notifications
* AWS-hosted automation server
* Production deployment to Linux server


# 2. My Accomplisments 

### **Eliminated A Critical Failure By Discovering a Technical Bug**

While setting up automated deployments from Jenkins to the web server, the deployment kept failing whenever Jenkins tried to use the stored SSH credentials. To troubleshoot the issue, I reviewed the error logs, checked the SSH agent plugin documentation, and explored the Jenkins file structure to understand how the plugin was creating temporary SSH files.

I discovered that the problem was caused by spaces in the Jenkins pipeline name. The plugin was generating file paths incorrectly because it could not properly handle the spaces, causing it to look for files that did not exist. I fixed the issue by renaming the project to remove spaces from the pipeline name.

### **Reduced Time Spent on Documentation by 50% Using AI-Assisted Workflows**

Applied advanced techniques such as iterative prompt refinement, asking AI to critique and improve prompts, and structuring prompts using markdown formatting to improve the response of AI tools.

The effective use of AI tools helped create first drafts for project documentation and README files, significantly reducing documentation time while maintaining clarity and structure.



### **Increased Long-Term Productivity By Complying with Best Practices**

According to Atlassia developers can lose up to 10 hours per week searching for missing or unclear information due to poor documentation practices (1). I created architecture diagrams and workflow diagrams to help future engineers quickly understand deployment flow and service dependencies without needing to manually inspect configuration files.

These improvements will make debugging easier, streamline onboarding for new contributors, and increase the long-term maintainability of the project. 

# 3. Architecture Diagram

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

# 4. References 

1. https://www.itpro.com/software/development/if-software-development-were-an-f1-race-these-inefficiencies-are-the-pit-stops-that-eat-into-lap-time-why-developers-need-to-sharpen-their-focus-on-documentation?utm_source=chatgpt.com