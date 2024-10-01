pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myapp .'
                sh 'docker run myapp'
            }
        }
    }
}

