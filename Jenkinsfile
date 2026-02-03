pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select the environment'
        )
    }

    environment {
        APP_NAME = 'the-paris-music'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from Git'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building application: ${env.APP_NAME}"
                echo "Target environment: ${params.ENV}"
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'echo Tests executed successfully'
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
            echo 'PIPELINE COMPLETED SUCCESSFULLY'
        }
        failure {
            echo 'PIPELINE FAILED'
        }
    }
}
