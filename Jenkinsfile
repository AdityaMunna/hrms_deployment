pipeline {
    agent { label 'Agent-Mohan' }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/mohancc1/hrms_deployment.git'
            }
        }

        stage('Build Test') {
            steps {
                sh 'echo "✅ CI/CD Pipeline is working fine!"'
            }
        }
    }
}
