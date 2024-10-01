pipeline {
    // Define the agent (or environment) in which the pipeline will run
    // Using a Node.js Docker image to run the Node.js app. Remove if not using Docker.
    agent {
        docker {
            image 'node:16'  // Use Node.js version 16 image
            label 'node-agent'  // Label for the Jenkins agent (optional)
        }
    }

    // Define environment variables. Credentials are stored in Jenkins and referenced here.
    environment {
        SNYK_TOKEN = credentials('snyk-token')  // Fetch Snyk token from Jenkins credentials
        GITHUB_TOKEN = credentials('github-token')  // Fetch GitHub token from Jenkins credentials
    }

    stages {
        stage('Checkout') {
            // This stage checks out the code from the GitHub repository
            steps {
                git branch: 'main', url: 'https://github.com/hirdyasharma/aws-elastic-beanstalk-express-js-sample'
            }
        }

        stage('Install Dependencies') {
            // In this stage, we install the Node.js dependencies using npm
            steps {
                sh 'npm install'  // Run 'npm install' to install dependencies defined in package.json
            }
        }

        stage('Run Tests') {
            // Here we run the tests for the Node.js application
            steps {
                sh 'npm test'  // Run 'npm test' to execute the defined test scripts
            }
        }

        stage('Snyk Security Scan') {
            // This stage performs a security scan using Snyk
            steps {
                sh 'npx snyk auth $SNYK_TOKEN'  // Authenticate with Snyk using the token
                sh 'npx snyk test'  // Run Snyk tests to check for vulnerabilities in dependencies
            }
        }
    }

    post {
        always {
            // This block runs after every build (success or failure)
            // JUnit test result collection (if applicable)
            junit 'test-results/*.xml'  // Collect test results from the specified path

            // Archive build artifacts so they can be viewed/downloaded later
            archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true  // Archive artifacts (like build files), even if none are found
        }

        failure {
            // If the build fails, send an email notification
            mail to: 'hirdyasharma95@gmail.com',  // Replace with your team's email
                 subject: "Build Failed: ${currentBuild.fullDisplayName}",  // Dynamic subject showing the build name and status
                 body: "Something went wrong. Please check the Jenkins logs for more details."  // Email body with a message to investigate
        }
    }
}

