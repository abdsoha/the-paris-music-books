pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Checkout') stage('Checkout') {
            steps {
                // Senior Practice: Don't hardcode URLs; inherit from the Job configuration
                checkout scm 
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t paris-music-app .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f paris-music || true
                docker run -d -p 9090:8080 --name paris-music paris-music-app
                '''
            }
        }

    }
}
