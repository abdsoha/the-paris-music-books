pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Choose environment'
        )
    }

    environment {
        APP_NAME = 'the-paris-music'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building ${env.APP_NAME} for ${params.ENV}"
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'echo Tests passed'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${env.APP_NAME} to ${params.ENV}"
            }
        }
    }

    post {
        success {
            echo 'Pipeline SUCCESS'
        }
        failure {
            echo 'Pipeline FAILED'
        }
    }
}
