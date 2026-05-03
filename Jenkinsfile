pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
       
        stage('Run Tests') {
            steps {    
                sh 'python3 ./tests/basic_tests.py'
            }
        } 
        stage('Push to Web Server') {
            steps { 
                sh "rm -rf ./.git"
                sh "rm -rf ./ci"
                sshagent(credentials: ['cslinux']) {
                    sh "scp -o StrictHostKeyChecking=no -r . enioluwafe.balogun@cslinux.ucalgary.ca:~/www"  
                }
            }
        } 
    }
    post {
        success { 
            emailext(
                to: 'enioluwafe.balogun@ucalgary.ca', 
                subject: "[SUCCESSFUL] Pipeline: ${env.JOB_NAME} (Build #${env.BUILD_NUMBER})",
                body: """
                  <h2>Pipeline Success Alert</h2>
                  <p>Job: ${env.JOB_NAME}</p>
                  <p>Build Number: ${env.BUILD_NUMBER}</p>
                  <p>Status: <b>SUCCESS</b></p>
                  <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                mimeType: 'text/html', 
      )
    } 
        failure {
            emailext(
                to: 'enioluwafe.balogun@ucalgary.ca', 
                subject: "[FAILED] Pipeline: ${env.JOB_NAME} (Build #${env.BUILD_NUMBER})",
                body: """
                  <h2>Pipeline Failure Alert</h2>
                  <p>Job: ${env.JOB_NAME}</p>
                  <p>Build Number: ${env.BUILD_NUMBER}</p>
                  <p>Status: <b>FAILED</b></p>
                  <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                mimeType: 'text/html', 
                attachmentsPattern: '**/build.log' 
            )
        }
    }
}
