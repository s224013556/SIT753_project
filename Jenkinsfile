pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository using stored credentials
                git branch: 'main', credentialsId: '26153eea-bc63-41ff-9038-6cf30315c9da', url: 'https://github.com/s224013556/SIT753_project.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    // Create a directory for artifacts if it doesn't exist
                    sh 'mkdir -p artifacts'
                    
                    // Get a list of files and directories to copy, excluding 'artifacts'
                    def items = sh(script: 'ls -A | grep -v artifacts', returnStdout: true).trim().split("\n")

                    // Copy each item to the artifacts directory
                    for (item in items) {
                        sh "cp -r ${item} artifacts/"
                        }

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
         stage('Deploy') {
            steps {
                // Use Docker Pipeline to deploy the application
                script {
                    docker.withServer('tcp://your-docker-host:2375') {
                        docker.image('your-application-image').run()
                    }
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
