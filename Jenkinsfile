pipeline {
    agent any

    tools {
        // Must match the name you configured in Manage Jenkins > Tools
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                // Inherits the Git URL and credentials from your Jenkins Job settings
                checkout scm 
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                // Uses the unique build number for senior-level version control
                sh "docker build -t paris-music-app:${env.BUILD_NUMBER} ."
                sh "docker tag paris-music-app:${env.BUILD_NUMBER} paris-music-app:latest"
            }
        }

        stage('Deploy Locally') {
            steps {
                sh '''
                docker stop paris-music || true
                docker rm paris-music || true
                docker run -d -p 9090:8080 --name paris-music paris-music-app:latest
                '''
            }
        }
    }

    post {
        always {
            echo "Build finished. Cleaning workspace to save disk space."
            cleanWs()
        }
    }
}
