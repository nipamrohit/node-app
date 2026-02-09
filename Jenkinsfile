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
                docker rm -f node-app 2>nul
                docker run -d --name node-app -p 3000:3000 nipamrohit121/node-app
                '''
            }
        }
    }

    post {
        failure {
            emailext(
                mimeType: 'text/html',
                subject: "❌ Jenkins Build Failed | ${JOB_NAME} #${BUILD_NUMBER}",
                body: """
                <h2 style="color:red;">❌ Build Failed</h2>
                <p><b>Project:</b> ${JOB_NAME}</p>
                <p><b>Build:</b> #${BUILD_NUMBER}</p>
                <p><b>Node:</b> ${NODE_NAME}</p>
                <p><a href="${BUILD_URL}">Build URL</a></p>
                <p><a href="${BUILD_URL}console">Console Logs</a></p>
                """,
                to: "nipamrohit121@gmail.com"
            )
        }
    }
}
