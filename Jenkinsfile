pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select environment'
        )
    }

    environment {
        SONAR_HOST_URL = 'http://136.113.252.182:9000'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/abdsoha/the-paris-music-books.git'
            }
        }

        stage('Build with Maven') {
            steps {
                dir('paris-music-app') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('SonarQube Code Analysis') {
            steps {
                dir('paris-music-app') {
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                        sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=paris-music-app \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully with SonarQube analysis'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
