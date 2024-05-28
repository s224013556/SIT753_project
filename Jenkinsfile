pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                // Your build commands here
                sh 'npm install' // Example for a Node.js project
            }
        }
        stage('Test') {
            steps {
                // Your test commands here
                sh 'npm test' // Example for running tests in a Node.js project
            }
        }
    }
    
    post {
        always {
            // Archive the build artifacts
            archiveArtifacts artifacts: 'build/**/*.zip' // Example: Archive all .zip files in the 'build' directory
        }
        success {
            // Send success notification
            echo 'Build successful! Sending notification...'
            // Example: Send a Slack notification
            slackSend channel: '#builds', color: 'good', message: 'Build successful!'
        }
        failure {
            // Send failure notification
            echo 'Build failed! Sending notification...'
            // Example: Send a Slack notification
            slackSend channel: '#builds', color: 'danger', message: 'Build failed!'
        }
    }
}
