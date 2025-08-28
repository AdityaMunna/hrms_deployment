pipeline {
    agent { label 'Agent-Mohan' }

    stages {
        stage('Git: Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mohancc1/hrms_deployment.git'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {  // go inside frontend folder
                    sh 'docker build -t hrms-frontend:latest .'
                }
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('Backend_hrms') {  // go inside backend folder
                    sh 'docker build -t my-backend-app:latest .'
                }
            }
        }

        stage('Run Containers') {
            steps {
                sh 'docker run -d -p 8080:80 my-frontend-app:latest'
                sh 'docker run -d -p 8000:8000 my-backend-app:latest'
            }
        }
    }
}
