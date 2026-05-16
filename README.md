# Portfolio Website CI/CD Pipeline

## 1. Project Overview

This project demonstrates a fully automated CI/CD pipeline for deploying a static portfolio website using modern DevOps tools and practices. The pipeline continuously monitors a GitHub repository for changes, automatically tests the website code, and deploys validated updates to a production Linux web server.

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
|   Developer       |
|                   |
+-------------------+
          |
          | Push Code
          v
+-------------------+
|      GitHub       |
|  SCM Repository   |
+-------------------+
          |
          | Webhook Trigger
          v
+-----------------------------+
|    AWS EC2 Ubuntu Server    |
|         Jenkins CI          |
| Python Tests + Deployment   |
+-----------------------------+
          |
          | SCP Deployment
          v
+-----------------------------+
|   School Linux Web Server   |
|       ~/www Directory       |
|  Production Portfolio Site  |
+-----------------------------+
```

---

# 3. Step 1: AWS EC2 Instance Preparation

This section explains how to create and configure an AWS EC2 Ubuntu instance to host Jenkins.

## Create an EC2 Instance

1. Sign in to AWS Management Console.
2. Navigate to EC2 Dashboard.
3. Click **Launch Instance**.
4. Configure the instance:

### Recommended Settings

| Setting       | Value                            |
| ------------- | -------------------------------- |
| AMI           | Ubuntu Server 22.04 LTS          |
| Instance Type | t2.micro (Free Tier Eligible)    |
| Storage       | 8 GB minimum                     |
| Key Pair      | Create or select an SSH key pair |

---

## Configure Security Groups

Jenkins must be reachable from GitHub webhooks and your browser.

### Inbound Rules

| Type       | Protocol | Port | Source    |
| ---------- | -------- | ---- | --------- |
| SSH        | TCP      | 22   | Your IP   |
| HTTP       | TCP      | 80   | 0.0.0.0/0 |
| Custom TCP | TCP      | 8080 | 0.0.0.0/0 |

### Why Port 8080?

Jenkins runs on port `8080` by default.

### Outbound Rules

Allow all outbound traffic so Jenkins can:

* Connect to GitHub
* Send emails
* Deploy files via SCP
* Install software packages

---

## Connect to EC2 via SSH

```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```

Update the system packages:

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 4. Step 2: Jenkins Installation and Setup

This section explains how to install Jenkins and Python on the Ubuntu EC2 server.

---

## Install Java

Jenkins requires Java.

```bash
sudo apt install openjdk-17-jdk -y
```

Verify installation:

```bash
java -version
```

---

## Install Jenkins

Add the Jenkins repository:

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Install Jenkins:

```bash
sudo apt update
sudo apt install jenkins -y
```

Start and enable Jenkins:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Check Jenkins status:

```bash
sudo systemctl status jenkins
```

---

## Install Python and Required Packages

Install Python:

```bash
sudo apt install python3 python3-pip -y
```

Install BeautifulSoup:

```bash
pip3 install beautifulsoup4
```

Install unittest support:

```bash
pip3 install pytest
```

---

## Access Jenkins

Open a browser:

```text
http://YOUR_EC2_PUBLIC_IP:8080
```

Retrieve the Jenkins admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Complete the Jenkins setup wizard and install recommended plugins.

---

# 5. Step 3: GitHub Repository Configuration

Repository URL:

```text
github.com/enibalo/portfolio-website
```

This step explains how to configure GitHub webhooks so GitHub can notify Jenkins whenever code changes occur.

---

## Install Required Jenkins Plugins

Inside Jenkins:

1. Go to **Manage Jenkins**
2. Select **Plugins**
3. Install:

* GitHub Integration Plugin
* Git Plugin
* Pipeline Plugin

Restart Jenkins after installation.

---

## Configure GitHub Webhook

In your GitHub repository:

1. Go to **Settings**
2. Select **Webhooks**
3. Click **Add Webhook**

### Webhook Settings

| Setting      | Value                                            |
| ------------ | ------------------------------------------------ |
| Payload URL  | `http://YOUR_EC2_PUBLIC_IP:8080/github-webhook/` |
| Content Type | `application/json`                               |
| Trigger      | Just the push event                              |

Save the webhook.

---

## Generate SSH Keys for Deployment

On the Jenkins server:

```bash
ssh-keygen -t rsa
```

Copy the public key:

```bash
cat ~/.ssh/id_rsa.pub
```

Add the public key to your school Linux server:

```bash
~/.ssh/authorized_keys
```

This allows Jenkins to securely deploy updates without entering a password.

---

## Jenkinsfile

Example Jenkins pipeline configuration:

```groovy
pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/enibalo/portfolio-website.git'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python3 test_portfolio.py'
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                scp -r index.html styles.css \
                username@school-server:~/www/
                '''
            }
        }
    }

    post {
        success {
            mail to: 'your-email@example.com',
            subject: 'Deployment Successful',
            body: 'Portfolio website deployed successfully.'
        }

        failure {
            mail to: 'your-email@example.com',
            subject: 'Deployment Failed',
            body: 'Jenkins pipeline failed.'
        }
    }
}
```

---

# 6. Step 4: Jenkins Pipeline Creation and Execution

This section explains how to create the Jenkins pipeline job.

---

## Create a Pipeline Job

1. Open Jenkins Dashboard
2. Click **New Item**
3. Enter a project name:

```text
portfolio-website-ci
```

4. Select **Pipeline**
5. Click **OK**

---

## Configure GitHub Repository

Under Pipeline settings:

### Repository URL

```text
https://github.com/enibalo/portfolio-website.git
```

---

## Configure GitHub Webhook Trigger

Under **Build Triggers**:

Enable:

```text
GitHub hook trigger for GITScm polling
```

---

## Configure SSH Credentials

1. Go to:

```text
Manage Jenkins → Credentials
```

2. Add new credentials:

| Setting     | Example                       |
| ----------- | ----------------------------- |
| Kind        | SSH Username with private key |
| Username    | randomSSHUser                 |
| Private Key | Paste generated private key   |

These credentials allow Jenkins to securely connect to the school Linux server during deployment.

---

## Execute the Pipeline

Push code to GitHub:

```bash
git add .
git commit -m "Updated portfolio website"
git push origin main
```

GitHub automatically triggers Jenkins through the webhook.

Jenkins will:

1. Pull the updated repository
2. Run Python tests
3. Validate HTML content
4. Deploy files to the production server
5. Send success or failure email notifications

---

# 7. Conclusion

This project demonstrates practical implementation of CI/CD automation using industry-standard DevOps tools such as Jenkins, GitHub, AWS EC2, and Linux servers. The automated testing and deployment workflow improves reliability by ensuring only validated code reaches production. The solution also highlights experience with cloud infrastructure, Linux administration, automation scripting, and secure deployment practices commonly used in real-world DevOps environments.

---

# 8. Work Flow Diagram

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
     +----+----+
     |         |
Tests Pass   Tests Fail
     |         |
     v         v
+----------------------+     +----------------------+
| SCP Deploy Website   |     | Send Failure Email   |
| to School Linux Host |     | Notification         |
+----------------------+     +----------------------+
          |
          | Update ~/www
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
