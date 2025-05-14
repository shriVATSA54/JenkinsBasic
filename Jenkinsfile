pipeline {
    agent any

    environment {
        PYTHON_VERSION = '3.11'
        IMAGE_NAME = 'shrivatsa54/flask-task-app'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Setup') {
            steps {
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
                withCredentials([usernamePassword(credentialsId: 'DockerHubCreds', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPass')]) {
                    script {
                        // Build the Docker image
                        bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."

                        // Login to Docker Hub
                        bat "echo %dockerHubPass% | docker login -u %dockerHubUser% --password-stdin"

                        // Push the image to Docker Hub
                        bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
                    }
                }
            }
        }
    }
}
