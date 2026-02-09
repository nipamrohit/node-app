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
        post {
            failure {
            emailext(
                mimeType: 'text/html',
                subject: "❌ Jenkins Build Failed | ${JOB_NAME} #${BUILD_NUMBER}",
                body: """
                <html>
                <body style="font-family: Arial, sans-serif;">
                <h2 style="color:red;">❌ Build Failed</h2>

                    <table border="1" cellpadding="8" cellspacing="0" style="border-collapse: collapse;">
                        <tr><td><b>Project</b></td><td>${JOB_NAME}</td></tr>
                        <tr><td><b>Build Number</b></td><td>#${BUILD_NUMBER}</td></tr>
                        <tr><td><b>Status</b></td><td style="color:red;"><b>FAILED</b></td></tr>
                        <tr><td><b>Triggered By</b></td><td>${BUILD_USER}</td></tr>
                        <tr><td><b>Node</b></td><td>${NODE_NAME}</td></tr>
                        <tr><td><b>Workspace</b></td><td>${WORKSPACE}</td></tr>
                        <tr><td><b>Build URL</b></td>
                            <td><a href="${BUILD_URL}">${BUILD_URL}</a></td>
                        </tr>
                        <tr><td><b>Console Logs</b></td>
                            <td><a href="${BUILD_URL}console">${BUILD_URL}console</a></td>
                        </tr>
                    </table>

                    <br>
                    <p>Please check the console logs for error details.</p>
                    <p style="color:gray;">Jenkins CI/CD Notification</p>
                </body>
                </html>
                """,
                to: "yourmail@gmail.com"
            )
        }
    }

    }

}
