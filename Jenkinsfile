pipeline {
    agent any

    parameters {
        string(name: 'TAG', defaultValue: 'latest', description: 'Tag for the Docker image')
        choice(name: 'ENV', choices: ['test', 'uat', 'dev', 'prod'], description: 'Environment')
    }

    environment {
        IMAGE_NAME = "mithundevopsaws/${params.ENV}:${params.TAG}"
        DOCKER_CREDENTIALS = credentials('docker-credentials-id')
        DOCKER_CREDENTIALS_PSW = credentials('docker-password-id')
    }

    stages {
        stage('Login to Docker Registry') {
            steps {
                script {
                    echo "Logging in to Docker registry"
                     sh 'echo %DOCKER_CREDENTIALS_PSW% | sudo docker login -u $DOCKER_CREDENTIALS --password-stdin'
                }
            }
        }

        stage('Build Docker Image ') {
            steps {
                script {
                    echo "Building Docker image: ${env.IMAGE_NAME}"
                    sh "sudo docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Push Image to Dockerhub') {
            steps {
                script {
                    echo "Pushing Docker image: ${env.IMAGE_NAME}"
                    sh "sudo docker push ${IMAGE_NAME}"
                }
            }
        }
    }
}
