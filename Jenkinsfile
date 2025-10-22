properties([
    pipelineTriggers([
        githubPush()
    ])
])

node {
    stage('Checkout') {
        checkout scm
    }

    stage('Preparation') {
        catchError(buildResult: 'SUCCESS') {
            sh 'docker stop samplerunning || true'
            sh 'docker rm samplerunning || true'
        }
    }

    stage('Build') {
        build 'BuildSampleApp'
    }

    stage('Results') {
        build 'TestSampleApp'
    }
}
