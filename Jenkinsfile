pipeline {
    agent { label 'Agent-Mohan' }
    stages {

 
        stage('Clone Frontend') {
            steps {
                dir('frontend') {
                    git branch: 'main', url: 'https://github.com/mohancc1/hrms_deployment.git'
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
                    sh 'docker build -t mohancc1/backend:latest .'
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
                    sh 'docker build -t mohancc1/frontend:latest .'
                }
            }
        }	
		 stage('Push Images to Docker Hub') {
            steps {
                echo 'Logging in and pushing image to Docker Hub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push mohancc1/frontend:latest'
                    sh 'docker push mohancc1/backend:latest'
                }
                echo 'Docker images pushed successfully.'
            }
        }

		 stage('Deploy with Docker Compose') 
		 {
            steps {
                echo 'Deploying application with Docker Compose...'
				sh '''
				docker compose down --volumes --remove-orphans
                docker system prune -af --volumes
				docker compose down
				docker compose up -d
				'''
                echo 'Application deployed successfully.'
            }
        }

		stage('Backup MySQL') {
    steps {
        script {
            sh '''
                set -e
                MAX_TRIES=36
                TRIES=0

                until docker exec djangocicd-mysql-1 mysqladmin ping -uroot -proot --silent; do
                  echo "Waiting for MySQL container to be ready..."
                  sleep 5
                  TRIES=$((TRIES+1))
                  if [ $TRIES -ge $MAX_TRIES ]; then
                    echo "MySQL did not start in 3 minutes!"
                    exit 1
                  fi
                done

                echo "MySQL is ready. Running backup..."
                docker exec djangocicd-mysql-1 sh -c 'exec mysqldump -uroot -proot hrms_db' > backup.sql
            '''
        }
        archiveArtifacts artifacts: 'backup.sql', fingerprint: true
    }
}


 
		

    }
}
    
