pipeline {
    agent {
        docker {
            image 'node:16'  // Use Node.js 16 Docker image as the build agent
            args '-u root:root'  // Run the container as root user for permissions
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token')  // Use the Snyk token from Jenkins credentials
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install project dependencies
                sh 'npm install --save'
            }
        }
        stage('Security Scan') {
            steps {
                // Run Snyk to scan for vulnerabilities
                script {
                    def snykScanResult = sh(script: 'snyk test --all-projects --json', returnStdout: true)
                    echo snykScanResult  // Print the Snyk scan results
                    def snykResultJson = readJSON text: snykScanResult
                    // Check for critical vulnerabilities and fail the build if found
                    if (snykResultJson.vulnerabilities.any { it.severity == 'critical' }) {
                        error "Critical vulnerabilities found! Halting the pipeline."
                    }
                }
            }
        }
    }
    post {
        always {
            // Archive the Snyk scan results for future reference
            archiveArtifacts artifacts: '**/snyk-results.json', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

