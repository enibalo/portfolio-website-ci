# Portfolio Website CI/CD Pipeline

### **Table of Contents**
1. [Project Overview](#1-project-overview)
3. [Architecture Diagram](#3-architecture-diagram)
4. [Workflow Diagram](#4-workflow-diagram)
2. [My Accomplishments](#2-my-accomplishments)

## 1. Project Overview

This project created a Jenkins pipeline for deploying updates to my cloud-hosted portfolio website. The Jenkins server was hosted on an AWS EC2 instance and continuously monitored [my portfolio websitet's git repository](https://github.com/enibalo/portfolio-website) for changes. When a change ws detected, it ran tests and deployed the tested updates to the University of Calgary Linux web server where my portfolio website is hosted.

Key technologies: Git, GitHub, Jenkins, AWS EC2 Server, Bash, Python, Ngrok (It was used for running the Jenkins server locally for testing.)

# 2. Architecture Diagram

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

# 3. Workflow Diagram

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
# Problem-Solving in Practice: Debugging an SSH Failure in the Pipeline

The pipeline failed whenever Jenkins tried to use the SSHAgent plugin to push validated changes to production.

Based on: 
1. Learning that the SSHAgent plugin created temporary SSH files by reading the documentation. 

2. Reading the error logs and exploring the Jenkins working directory.

I discovered that the problem was caused by spaces in the Jenkins pipeline name. The name of the pipeline was "Portfolio Pipline", and Jenkins used this name to create files and directories. The error log revealed that the SSHAgent plugin was generating file paths incorrectly because it could not handle files/directories that contained spaces. I fixed the issue by renaming the project to "Portfolio-Pipeline".

#  AI-Assisted Workflows: Reduced Time Spent on Documentation by 50% Using
Used AI tools to create first drafts for project documentation and README files, significantly reducing documentation time while maintaining clarity and structure.

Applied advanced prompt refinement techniques such as:
1. Iterative improvement.
2. Asking AI to critique and improve prompts.
3. Structuring prompts using markdown formatting.

# Project Setup 

The Jenkins file was placed inside a 'ci' folder in my portfoloio website repository: 

