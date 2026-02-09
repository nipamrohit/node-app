pipeline {
    agent any

    environment {
        IMAGE_NAME = "nipamrohit121/node-app"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    """
                }
            }
        }
    }

    post {
        failure {
            emailext(
                subject: "❌ Build Failed: ${JOB_NAME} #${BUILD_NUMBER}",
                body: """
                Build Failed<br>
                Job: ${JOB_NAME}<br>
                Build: ${BUILD_NUMBER}<br>
                URL: ${BUILD_URL}
                """,
                to: "nipamrohit121@gmail.com"
            )
        }
    }
}
