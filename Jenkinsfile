pipeline {
    agent { label 'Agent-Mohan' }

    stages {
        stage('Git: Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mohancc1/hrms_deployment.git'
            }
        }

        stage('Docker Compose: Build & Run') {
            steps {
                // Make sure Docker Compose file is at the root OR adjust the path
                sh 'docker-compose down --remove-orphans'
                sh 'docker-compose up --build -d'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up containers...'
            sh 'docker-compose down --remove-orphans'
        }
    }
}
