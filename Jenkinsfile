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
                sh 'docker build -t nipamrohit121/node-app .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push nipamrohit121/node-app'
            }
        }
    }
}
