pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // or use GitHub webhook
    }

    environment {
        APP_NAME = 'samplerunning'
        IMAGE_NAME = 'sampleapp:latest'
    }

    stages {
        stage('Cleanup') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS') {
                        sh """
                        docker stop ${APP_NAME} || true
                        docker rm ${APP_NAME} || true
                        """
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Test') {
            steps {
                echo 'Testing image...'
                sh "docker run --rm ${IMAGE_NAME} npm test || true"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container...'
                sh "docker run -d --name ${APP_NAME} -p 8080:80 ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
