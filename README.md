# Portfolio Website CI/CD Pipeline 

## Table of Contents
- [Project Overview](#project-overview)
- [Workflow Diagram](#workflow-diagram)
- [Problem-Solving in Practice: Debugging an SSH failure in the Pipeline](#problem-solving-in-practice-debugging-an-ssh-failure-in-the-pipeline)
- [AI-Assisted Workflows: Reducing The Time Spent on Documentation by 50% ](#ai-assisted-workflows-reduced-time-spent-on-documentation-by-50-using)
- [Project Setup](#project-setup)

## Project Overview

This project created a Jenkins pipeline for deploying updates to my cloud-hosted portfolio website. The Jenkins server was hosted on an AWS EC2 instance and continuously monitored my portfolio website's git repository for pushes to `main`. When a change to the `main` branch was detected, it ran tests and deployed the tested updates to the University of Calgary Linux web server, where my portfolio website is hosted in the folder `/home/csusers/enioluwafe.balogun/www`.

Key technologies: Git, GitHub, Jenkins, AWS EC2 Server, Bash (the pipeline executed the steps I wrote in Bash commands), Python (used to create the unit tests), and Ngrok (used for running the Jenkins server locally for testing).

## Workflow Diagram

![Flowchart of a Jenkins CI/CD pipeline: developer pushes code to GitHub, which triggers Jenkins to clone the repo and run unit tests. If tests pass, the site is deployed via SCP to the school Linux server and the developer is notified. If tests fail, a failure email is sent instead.](/images/workflow-diagram.svg)

## Problem-Solving in Practice: Debugging an SSH Failure in the Pipeline

The pipeline failed whenever Jenkins tried to use the SSHAgent plugin to push validated changes to production.

Based on:
1. Deciding to read the SSHAgent documentation and learning that the SSHAgent plugin created temporary SSH files.
2. Reading the error logs and exploring the Jenkins working directory.

I discovered that the problem was caused by spaces in the Jenkins pipeline name. The pipeline was named "Portfolio Pipeline," and the SSHAgent plugin was using this name to create its SSH files but wasn't escaping spaces correctly, so it always ended up trying to reference a file that did not exist. I fixed the issue by renaming the project to "Portfolio-Pipeline."

## AI-Assisted Workflows: Reducing the Time Spent on Documentation by 50%

I used AI tools to create first drafts of project documentation and README files, significantly reducing documentation time while maintaining clarity and structure.

I applied advanced prompt refinement techniques such as:
1. Iterative improvement.
2. Asking AI to critique and improve prompts.
3. Structuring prompts using Markdown formatting.

## Project Setup

A rundown of the high-level steps I used to create this project:

1. I created a `ci` folder in my portfolio website that contained the Jenkinsfile, which defined the pipeline's steps, and a `tests` folder that contained the Python tests I wanted to run.

```
portfolio-website/
├── ci/
│   └── Jenkinsfile
├── css/
├── images/
├── tests/
│   └── basic_tests.py
├── index.html
├── LICENSE
└── README.md
```

2. I created an AWS EC2 instance and installed Jenkins on it.

3. I configured the AWS EC2 instance to allow outgoing and incoming traffic on port 8080 so that the Jenkins server could receive events via a GitHub webhook.

4. I created a GitHub webhook for my portfolio website repository. I configured it to trigger on push events and to send those events to the AWS EC2 Jenkins server.

5. I created a GitHub Web Token for my Jenkins server so it could authenticate with GitHub. I also created a Google App Password so that Jenkins could authenticate with Google and send Gmail notifications.

6. I started the Jenkins server on my EC2 instance and added the GitHub Web Token credentials. I also added the Gmail credentials to the Jenkins server, as well as the SSHAgent plugin so I could use `scp` to forward changes to the school's server, and the EmailExt plugin so I could send email notifications at the end of my pipeline.

7. I created a new pipeline on the Jenkins server called "Portfolio-Pipeline" and configured it to trigger on a push to my portfolio website repository, to use my GitHub Web Token credentials to authenticate with GitHub, and to execute the steps in the `ci/Jenkinsfile` once triggered. The `Jenkinsconfig.xml` file contains the pipeline's exact configuration.
