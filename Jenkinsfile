pipeline {
    agent any
    environment {
        PYTHON_VERSION = '3.11' // Use a compatible Python version
    }
    stages {
        stage('Setup') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubCreds', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                    bat '''
                    echo ${myuser}
                    echo ${mypassword}
                    '''
                }
                bat 'pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                bat 'pytest'
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
