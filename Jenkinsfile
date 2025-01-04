pipeline {
    agent any

    parameters {
        string(name: 'TAG', defaultValue: 'latest', description: 'Tag for the Docker image')
        choice(name: 'ENV', choices: ['test', 'uat', 'dev', 'prod'], description: 'Environment')
    }

    environment {
        IMAGE_NAME = "myapp-${params.ENV}:${params.TAG}"
        DOCKER_REGISTRY = credentials('docker-registry-url')
        DOCKER_CREDENTIALS = credentials('docker-credentials-id')
    }

    stages {
        stage('Login to Docker Registry') {
            steps {
                script {
                    echo "Logging in to Docker registry"
                    sh "echo ${DOCKER_CREDENTIALS_PSW} | docker login ${DOCKER_REGISTRY} -u ${DOCKER_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building Docker image: ${env.IMAGE_NAME}"
                    sh "docker build -t ${env.IMAGE_NAME} ."
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    echo "Pushing Docker image: ${env.IMAGE_NAME}"
                    sh "docker push ${env.IMAGE_NAME}"
                }
            }
        }
    }
}
