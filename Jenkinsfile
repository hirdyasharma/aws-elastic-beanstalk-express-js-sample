pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root'  // Run as root to install Git and necessary dependencies
        }
    }

    environment {
        SNYK_TOKEN = credentials('snyk-token')  // Use your Snyk token stored in Jenkins credentials
    }

    stages {
        stage('Install Git') {
            steps {
                script {
                    // Update package manager and install Git in the container
                    sh 'apt-get update && apt-get install -y git'
                }
            }
        }

        stage('Checkout Code') {
            steps {
                // Checkout the code from your GitHub repository
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install the necessary Node.js dependencies
                    sh 'npm install --save'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Run your tests (ensure you have a valid 'npm test' command set up in package.json)
                    sh 'npm test'
                }
            }
        }
        
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Install Snyk globally, authenticate with the token, and perform a security scan
                    sh 'npm install -g snyk'
                    sh 'snyk auth $SNYK_TOKEN'
                    sh 'snyk test'
                }
            }
        }
    }

    post {
        failure {
            // In case of failure, notify with a message
            echo 'Pipeline failed!'
        }
        success {
            // On success, print this message
            echo 'Pipeline completed successfully!'
        }
    }
}

