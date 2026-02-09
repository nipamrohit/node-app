pipeline {
    agent any

    environment {
        IMAGE_NAME = "nipamrohit121/node-app"
        CONTAINER_NAME = "node-app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
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
                    docker push %IMAGE_NAME%
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                docker rm -f %CONTAINER_NAME% 2>nul
                docker run -d --name %CONTAINER_NAME% -p 3000:3000 %IMAGE_NAME%
                '''
            }
        }
    }

    post {
        failure {
            emailext(
                mimeType: 'text/html',
                subject: "❌ Jenkins CI/CD Failure | ${JOB_NAME} #${BUILD_NUMBER}",
                body: """
                <html>
                <body style="font-family: Arial, sans-serif; background-color:#f4f6f8; padding:20px;">
                    <div style="max-width:700px; background:#ffffff; padding:20px; border-radius:8px;">

                        <h2 style="color:#d9534f;">❌ Build Failed</h2>
                        <p>The Jenkins pipeline execution has <b>FAILED</b>. Details are below:</p>

                        <table width="100%" cellpadding="8" cellspacing="0"
                               style="border-collapse:collapse; border:1px solid #ddd;">
                            <tr style="background:#f0f0f0;">
                                <td><b>Project Name</b></td>
                                <td>${JOB_NAME}</td>
                            </tr>
                            <tr>
                                <td><b>Build Number</b></td>
                                <td>#${BUILD_NUMBER}</td>
                            </tr>
                            <tr style="background:#f0f0f0;">
                                <td><b>Build Status</b></td>
                                <td style="color:#d9534f;"><b>FAILED</b></td>
                            </tr>
                            <tr>
                                <td><b>Executed On Node</b></td>
                                <td>${NODE_NAME}</td>
                            </tr>
                            <tr style="background:#f0f0f0;">
                                <td><b>Workspace</b></td>
                                <td>${WORKSPACE}</td>
                            </tr>
                            <tr>
                                <td><b>Docker Image</b></td>
                                <td>${IMAGE_NAME}</td>
                            </tr>
                            <tr style="background:#f0f0f0;">
                                <td><b>Build URL</b></td>
                                <td><a href="${BUILD_URL}">${BUILD_URL}</a></td>
                            </tr>
                            <tr>
                                <td><b>Console Logs</b></td>
                                <td><a href="${BUILD_URL}console">${BUILD_URL}console</a></td>
                            </tr>
                        </table>

                        <br>

                        <p style="color:#555;">
                            👉 Please review the console logs to identify the root cause of the failure.
                        </p>

                        <hr>

                        <p style="font-size:12px; color:#999;">
                            Jenkins CI/CD Automated Notification<br>
                            This is an auto-generated email. Do not reply.
                        </p>
                    </div>
                </body>
                </html>
                """,
                to: "nipamrohit121@gmail.com"
            )
        }
    }
}
