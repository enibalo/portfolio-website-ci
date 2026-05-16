# Portfolio Website CI/CD Pipeline

## 1. Project Overview

This independent project demonstrates a fully automated CI/CD pipeline for deploying a static portfolio website using modern DevOps tools and practices. The pipeline continuously monitors a GitHub repository for changes, automatically tests the website code, and deploys validated updates to a production Linux web server. The portfolio website can be found here: https://cspages.ucalgary.ca/~enioluwafe.balogun/.

### Technologies Used

* Git & GitHub
* Jenkins
* AWS EC2
* Ubuntu Linux
* Python 
    * BeautifulSoup
* SCP / SSH
* HTML & CSS

### Key Features

* Automated CI/CD deployment pipeline
* GitHub webhook integration
* Automated HTML validation testing
* Jenkins email notifications
* AWS-hosted automation server
* Production deployment to Linux server


# 2. My Accomplishments

### **Eliminated A Critical Pipeline Failure By Effectively Troubleshooting An Error**

The pipeline failed whenever Jenkins tried to use the SSHAgent plugin to push validated changes to production.

Based on: 
1. Learning that the SSHAgent plugin created temporary SSH files by reading the documentation. 

2. Reading the error logs and exploring the Jenkins working directory.

I discovered that the problem was caused by spaces in the Jenkins pipeline name. The name of the pipeline was "Portfolio Pipline", and Jenkins used this name to create files and directories. The error log revealed that the SSHAgent plugin was generating file paths incorrectly because it could not handle files/directories that contained spaces. I fixed the issue by renaming the project to "Portfolio-Pipeline".

### **Reduced Time Spent on Documentation by 50% Using AI-Assisted Workflows**

Used AI tools to create first drafts for project documentation and README files, significantly reducing documentation time while maintaining clarity and structure.

Applied advanced prompt refinement techniques such as:
1. Iterative improvement.
2. Asking AI to critique and improve prompts.
3. Structuring prompts using markdown formatting.


### **Increased Long-Term Productivity By Complying with Best Practices**

According to Atlassian's 2025 State of DevEx report, developers can lose up to 10 hours per week searching for missing or unclear information due to poor documentation practices (1). I created an architecture diagram and a workflow diagram to help future engineers quickly understand deployment flow and service dependencies without needing to manually inspect configuration files.

These improvements will make debugging easier, streamline onboarding for new contributors, and increase the long-term maintainability of the project. 

# 3. Architecture Diagram

```text
+-------------------+
|     Developer     |
+-------------------+
        |
        | 
        v
+-------------------+
|      GitHub       |
+-------------------+
        |
        | 
        v
+--------------------------------------+
|     AWS EC2 Ubuntu Linux Server      |
|  +------------------------------+    |
|  |        Jenkins Server        |    |
|  |                              |    |
|  |      Unit tests: Python      |    |
|  +------------------------------+    |
|                                      |
+--------------------------------------+
          |
          | 
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
+--------------------------+
| Jenkins Detects A Change |
+--------------------------+
          |
          | 
          v
+--------------------------+
| Jenkins Pipeline Starts  |
+--------------------------+
          |  Clone Repository
          |
          v
+------------------------+
|    Jenkins Executes    |
|       Unit Tests       |
+------------------------+
          |
     +----+-----------------------------+
     |                                  |
Tests Pass                          Tests Fail
     |                                  |
     v                                  v
+------------------------+      +----------------------+
|   SCP Deploy Website   |      |  Send Failure Email  |
| to School Linux Server |      |     Notification     |
+------------------------+      +----------------------+
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
|   Deployment Notification  |
|      Sent to Developer     |
+----------------------------+
```

# 4. References 

1. https://www.itpro.com/software/development/if-software-development-were-an-f1-race-these-inefficiencies-are-the-pit-stops-that-eat-into-lap-time-why-developers-need-to-sharpen-their-focus-on-documentation?utm_source=chatgpt.com