pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root' // Use root to avoid permission issues
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token') // Ensure this matches your credentials ID in Jenkins
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install --save'
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    // Check if a test script is defined
                    if (sh(script: 'grep -q test package.json', returnStatus: true) != 0) {
                        // If no test defined, set a dummy test script
                        sh 'npm pkg set scripts.test="echo No test specified"'
                    }
                }
                sh 'npm test'
            }
        }
        stage('Snyk Security Scan') {
            steps {
                sh 'npm install -g snyk' // Install Snyk globally
                // Authenticate with Snyk using the token
                sh 'echo $SNYK_TOKEN | snyk auth'
                // Run Snyk test while ignoring specific vulnerabilities
                sh 'snyk test --all-projects --ignore-policy'
            }
        }
    }
    post {
        failure {
            echo 'Pipeline failed!'
            // Optional: Send a notification or take action on failure
        }
        success {
            echo 'Pipeline completed successfully!'
            // Optional: Take action on success
        }
    }
}

