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

                bat "pip install -r requirements.txt"
            
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