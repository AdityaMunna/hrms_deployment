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
    
