pipeline {
    agent {
        docker {
            image 'node:16'  // Use Node 16 Docker image as the build agent
            args '-u root'   // Run as root user to avoid permission issues
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token') // Add your Snyk token from Jenkins credentials
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout code from GitHub
                    checkout scm
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    // Install project dependencies
                    sh 'npm install'
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Run Snyk to check for vulnerabilities
                    sh 'snyk test --all-projects --severity-threshold=critical'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // You can add your build steps here (if any)
                    echo 'Build step can be added here'
                }
            }
        }
    }
    post {
        always {
            script {
                // Archive logs regardless of success or failure
                echo 'Archiving logs...'
                archiveArtifacts artifacts: '**/logs/*', allowEmptyArchive: true
            }
        }
        failure {
            // If the build fails, notify
            echo 'Build failed!'
        }
        success {
            // If successful, notify
            echo 'Build completed successfully!'
        }
    }
}

