pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select environment'
        )
    }

    environment {
        SONAR_HOST_URL = 'http://http://136.113.252.182:9000'
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/abdsoha/the-paris-music-books.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Code Analysis') {
            steps {
                sh """
                mvn sonar:sonar \
                -Dsonar.projectKey=paris-music-app \
                -Dsonar.host.url=$SONAR_HOST_URL \
                -Dsonar.login=$SONAR_TOKEN
                """
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
