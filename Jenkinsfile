pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app'
        DOCKERHUB_REPO = 'yourdockerhubusername/flask-app'
        IMAGE_TAG = 'latest'
        COMMIT_HASH = ''
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

        stage('Build Docker Image') {
            steps {
                script {
                    // Get Git commit hash
                    COMMIT_HASH = bat(script: 'git rev-parse --short HEAD', returnStdout: true).trim()

                    // Check if local image with tag exists
                    def localImage = bat(script: "docker images -q ${IMAGE_NAME}:${COMMIT_HASH}", returnStdout: true).trim()
                    if (localImage) {
                        echo "Image for commit ${COMMIT_HASH} already exists locally. Skipping build."
                    } else {
                        bat "docker build -t ${IMAGE_NAME}:${COMMIT_HASH} ."
                        bat "docker tag ${IMAGE_NAME}:${COMMIT_HASH} ${DOCKERHUB_REPO}:${IMAGE_TAG}"
                        echo "Docker image built and tagged successfully."
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubCreds', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                    script {
                        def imageExistsInDockerHub = bat(script: "docker manifest inspect ${DOCKERHUB_REPO}:${IMAGE_TAG}", returnStatus: true)
                        if (imageExistsInDockerHub == 0) {
                            echo "Image already exists in DockerHub. Skipping push."
                        } else {
                            bat "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                            bat "docker push ${DOCKERHUB_REPO}:${IMAGE_TAG}"
                            echo "Image pushed to DockerHub."
                        }
                    }
                }
            }
        }
    }
}
