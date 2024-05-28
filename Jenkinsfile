pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository using stored credentials
                git credentialsId: 'github-credentials', url: 'https://github.com/s224013556/SIT753_project.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    // Create a directory for artifacts if it doesn't exist
                    sh 'mkdir -p artifacts'
                    
                    // Copy HTML, CSS, and image files to the artifacts directory
                    sh 'cp -r *.html *.css images/ artifacts/'
                }
                
                // Archive the artifacts
                archiveArtifacts artifacts: 'artifacts/**/*', allowEmptyArchive: true
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
