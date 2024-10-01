pipeline {
    agent {
        docker {
            image 'node:16' // Use Node 16 Docker image as the build agent
            args '-u root' // Run as root for permissions if needed
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token') // Retrieve the Snyk token from Jenkins credentials
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Run npm install to install dependencies
                    sh 'npm install --save'
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Run Snyk to test for vulnerabilities
                    sh 'npx snyk test --all-projects'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // You can add additional build steps here if needed
                    sh 'npm run build'
                }
            }
        }
    }
    post {
        failure {
            // Send notification or handle failure scenario
            echo 'Pipeline failed due to critical vulnerabilities or build issues.'
        }
        success {
            // Actions to take on success, like notifications or further deployments
            echo 'Pipeline completed successfully!'
        }
    }
}

