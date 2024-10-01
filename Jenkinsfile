pipeline {
    agent any
    stages {
        stage('Test Docker') {
            steps {
                script {
                    // Try to run a simple Docker command
                    sh 'docker --version'  // Check Docker version
                }
            }
        }
    }
}

