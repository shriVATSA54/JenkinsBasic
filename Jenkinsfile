pipeline {
    agent any

    stages {


        stage('Setup') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubCreds', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {

                    bat '''
                    echo ${myuser}
                    echo ${mypassword}
                    '''
                }
                 bat '''
                    python -m venv venv
                    call venv\\Scripts\\activate
                    pip install grpcio==1.48.2
                    pip install -r requirements.txt --no-deps
                    '''
               
            
            }
        }
        stage('Test') {
            steps {
                sh "pytest"
                
            }
        }    
        stage('Deployment') {
            input {
                message "Do you want to proceed further?"
                ok "Yes"
            }
            steps {
                echo "Running Deployment"
                
            }
        } 
        
            
    }
}
