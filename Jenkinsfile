pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root:root' // Ensure you have permission to install packages
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token') // Replace with your Snyk token ID in Jenkins
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
                    // Add a test script if not already present
                    if (sh(script: "grep -q 'test' package.json", returnStatus: true) != 0) {
                        sh "npm set-script test 'echo \"No test specified\"'"
                    }
                }
                sh 'npm test'
            }
        }
        stage('Snyk Security Scan') {
            steps {
                sh 'npm install -g snyk'
                sh "snyk auth ${SNYK_TOKEN}" // Authenticate using the Snyk token
                sh 'snyk test' // Run the Snyk security scan
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

