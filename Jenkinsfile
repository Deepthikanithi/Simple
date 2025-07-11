pipeline {
    agent any
 
    environment {
        IMAGE_NAME = 'synap-mentor'
        IMAGE_TAG = 'latest'
    }
 
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
 
        stage('Install Dependencies') {
            steps {
                dir('synap-mentor') {
                    bat 'npm ci'
                }
            }
        }
 
        stage('Build React App') {
            steps {
                dir('synap-mentor') {
                    bat 'set CI=false && npm run build'
                }
            }
        }
 
        stage('Build Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-credentials',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    dir('synap-mentor') {
                        bat 'docker build -t %DOCKER_USERNAME%/%IMAGE_NAME%:%IMAGE_TAG% .'
                    }
                }
            }
        }
 
        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    bat '''
                        echo Logging in...
                        echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
                        docker push %DOCKER_USERNAME%/%IMAGE_NAME%:%IMAGE_TAG%
                    '''
                }
            }
        }
    }
}