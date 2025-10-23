properties([
    pipelineTriggers([
        githubPush()
    ])
])

pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Preparation') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS') {
                        sh 'docker stop samplerunning || true'
                        sh 'docker rm samplerunning || true'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                build 'BuildSampleApp'
            }
        }

        stage('Results') {
            steps {
                build 'TestSampleApp'
            }
        }
    }
}