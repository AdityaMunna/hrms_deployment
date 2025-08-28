pipeline {
    agent { label 'Agent-Mohan' }

environment {
        DOCKER_IMAGE = "my-frontend-app"   // change to frontend / backend accordingly
        DOCKER_TAG   = "latest"
        APP_PORT     = "8000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/mohancc1/hrms_deployment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Test Image') {
            steps {
                script {
                    sh "docker run --rm -d -p ${APP_PORT}:8000 --name test_container ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "sleep 10"  // wait for app to boot
                    sh "curl -f http://localhost:${APP_PORT} || exit 1"
                    sh "docker stop test_container"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // run container in background permanently
                    sh """
                        docker stop ${DOCKER_IMAGE} || true
                        docker rm ${DOCKER_IMAGE} || true
                        docker run -d -p ${APP_PORT}:8000 --name ${DOCKER_IMAGE} ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
    }
}
