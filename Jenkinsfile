pipeline {
    agent any

    stages {
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
                    docker push nipamrohit121/node-app
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                docker run -d --name node-app -p 3000:3000 nipamrohit121/node-app
                '''
            }
        }        
    }
    
    post {
    failure {
        emailext (
            subject: "❌ Jenkins Build Failed: ${JOB_NAME} #${BUILD_NUMBER}",
            body: """
                <h3>Build Failed</h3>
                <p><b>Project:</b> ${JOB_NAME}</p>
                <p><b>Build:</b> #${BUILD_NUMBER}</p>
                <p><b>Status:</b> FAILED</p>
                <p><b>URL:</b> ${BUILD_URL}</p>
            """,
            to: "nipamrohit121@gmail.com"
        )
    }
}

}
