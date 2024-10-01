pipeline {
    agent {
        docker {
            image 'node:16'  // Use Node.js 16 image as build agent
        }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install --save'  // Install project dependencies
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    // Integrate Snyk for vulnerability scanning
                    sh 'snyk test'  
                }
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to critical vulnerabilities.'
            // You can add additional steps to halt the pipeline if needed
        }
    }
}

