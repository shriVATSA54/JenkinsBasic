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

                 bat 'python -m venv venv'
                    bat 'venv\\Scripts\\activate && python -m pip install --upgrade pip'

                     bat 'venv\\Scripts\\activate && pip install --only-binary=grpcio -r requirements.txt'
            
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
