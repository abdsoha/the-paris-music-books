pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling code from Git repository'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'This is a simple build step'
                ls -la
            }
        }

        stage('Test') {
            steps {
                echo 'Running basic test simulation'
                echo 'Tests passed successfully'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying The Paris Music website'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
