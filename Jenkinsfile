
pipeline {
    agent {label 'jenkins-agent'}
    
    stages {
    
        stage("Workspace cleanup"){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        
        stage('Git: Code Checkout') {
            steps {
                script{
                    code_checkout("https://github.com/mohancc1/hrms_deployment.git","main")
                }
            }
        }
        
       
        }
    }
}
