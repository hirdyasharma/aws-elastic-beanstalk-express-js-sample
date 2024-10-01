pipeline {
    agent {
        docker {
            image 'node:16' // Use Node 16 Docker image as the build agent
            args '-u root:root' // Optional: run as root user if needed
        }
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout code from SCM (Git)
                    try {
                        checkout scm
                    } catch (Exception e) {
                        error("Git checkout failed: ${e.message}")
                    }
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    try {
                        // Install project dependencies
                        sh 'npm install --save'
                    } catch (Exception e) {
                        error("Failed to install dependencies: ${e.message}")
                    }
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    try {
                        // Run Snyk security scan
                        sh 'snyk test --all-projects'
                    } catch (Exception e) {
                        error("Security scan failed: ${e.message}")
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
        failure {
            echo 'Build failed due to errors in stages.'
        }
    }
}

