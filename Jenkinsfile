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
                // Checkout code from GitHub
                git branch: 'main', url: 'https://github.com/hirdyasharma/aws-elastic-beanstalk-express-js-sample'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install project dependencies
                sh 'npm install'
            }
        }
        stage('Snyk Security Scan') {
            steps {
                // Run Snyk to check for vulnerabilities
                sh 'snyk test --all-projects --severity-threshold=critical'
            }
        }
        stage('Build') {
            steps {
                // You can add your build steps here (if any)
                echo 'Build step can be added here'
            }
        }
    }
    post {
        failure {
            // If the build fails, archive the logs
            echo 'Build failed, archiving logs...'
            archiveArtifacts artifacts: '**/logs/*', allowEmptyArchive: true
        }
        success {
            // If successful, notify
            echo 'Build completed successfully!'
        }
    }
}

