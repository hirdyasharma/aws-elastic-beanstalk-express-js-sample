pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root --volume $WORKSPACE:/workspace --workdir /workspace'  // Map Jenkins workspace to Docker and set it as working directory
        }
    }
    
    environment {
        SNYK_TOKEN = credentials('snyk-token')  // Use Snyk token from Jenkins credentials
    }
    
    stages {
        stage('Install Git') {
            steps {
                script {
                    sh 'apt-get update && apt-get install -y git'
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install --save'
                }
            }
        }
        
        stage('Add Test Script to package.json') {  // Add a simple test script if missing
            steps {
                script {
                    sh '''
                    if ! grep -q '"test":' package.json; then
                      npm set-script test "echo 'No test specified'"
                    fi
                    '''
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
        
        stage('Snyk Security Scan') {
            steps {
                script {
                    sh 'npm install -g snyk'
                    sh 'snyk auth $SNYK_TOKEN'
                    sh 'snyk test'
                }
            }
        }
    }
    
    post {
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}

