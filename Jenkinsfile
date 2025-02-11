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
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\activate && python -m pip install --upgrade pip'
                bat 'venv\\Scripts\\activate && pip install --only-binary=:all: grpcio grpcio-tools'
                bat 'venv\\Scripts\\activate && pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                bat 'venv\\Scripts\\activate && pytest'
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
