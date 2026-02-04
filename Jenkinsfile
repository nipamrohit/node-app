pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/nipamrohit/node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t nipamrohit121/node-app .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push nipamrohit121/node-deploy-app
                    '''
                }
            }
        }
    }
}
