pipeline {
    agent {
        docker {
            image 'node:16'  // Use Node.js 16 Docker image
            args '-u root:root' // Run as root if necessary
        }
    }
    environment {
        GITHUB_TOKEN = credentials('github-token') // This matches the ID of the GitHub token
        SNYK_TOKEN = credentials('snyk-token')     // This matches the ID of the Snyk token
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository using the GitHub token
                script {
                    sh '''
                    git clone https://$GITHUB_TOKEN@github.com/aws-samples/aws-elastic-beanstalk-express-js-sample.git
                    cd aws-elastic-beanstalk-express-js-sample
                    '''
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                sh 'npm install --save'
            }
        }
        stage('Snyk Vulnerability Scan') {
            steps {
                // Run Snyk to scan for vulnerabilities
                sh 'snyk test --all-projects'
            }
        }
    }
    post {
        failure {
            // Fail the build if Snyk finds vulnerabilities
            error 'Build failed due to critical vulnerabilities.'
        }
    }
}
