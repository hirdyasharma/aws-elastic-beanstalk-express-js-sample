pipeline {
    agent {
        docker {
            image 'node:16'  // Specify the Docker image here
            args '-u root'  // Run as root if permissions issue
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Checkout your SCM (e.g., Git)
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install --save'  // Install project dependencies
            }
        }
        // Add other stages as necessary
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true  // Adjust this line based on your project structure
        }
    }
}

