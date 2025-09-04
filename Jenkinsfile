pipeline {
    agent { label 'AGENT-ADITYA' }
    stages {

 
        stage('Clone Frontend') {
            steps {
                dir('frontend') {
                    git branch: 'main', url: 'https://github.com/AdityaMunna/hrms_deployment.git'
                }
            }
        }
 
            stage('Build Backend') 
		{
           steps 
		   {
               dir('Backend_hrms') 
			   {
                    sh 'mvn clean package -DskipTests'
               }
           } 
         }

		 stage('Build Backend Docker Image') 
		 {
            steps 
			{
                dir('Backend_hrms') 
				{
                    sh 'docker build -t adityatest728/backend:latest .'
                }
            }
         } 

		  stage('Build Frontend') 
		{
            steps 
			{
                echo 'Building Angular application...'
				 dir('frontend') 
				 {
		        sh '''  
		        rm -rf node_modules package-lock.json
                npm install
                ng build --configuration production --no-prerender
                '''
				}
            }
        } 

		 stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    sh 'docker build -t adityatest728/frontend:latest .'
                }
            }
        }	
		 stage('Push Images to Docker Hub') {
            steps {
                echo 'Logging in and pushing image to Docker Hub...'
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push adityatest728/frontend:latest'
                    sh 'docker push adityatest728/backend:latest'
                }
                echo 'Docker images pushed successfully.'
            }
        }

		 stage('Deploy with Docker Compose') 
		 {
            steps {
                echo 'Deploying application with Docker Compose...'
				sh '''
				
				docker compose up -d
				'''
                echo 'Application deployed successfully.'
            }
        }

			

    }
}
    
