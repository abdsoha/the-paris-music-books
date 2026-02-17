pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm 
            }
        }

        stage('Build') {
            steps {
                // Senior Practice: Use 'dir' to change into the project subdirectory
                dir('paris-music-app') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Docker Build') {
            steps {
                dir('paris-music-app') {
                    sh "docker build -t paris-music-app:${env.BUILD_NUMBER} ."
                    sh "docker tag paris-music-app:${env.BUILD_NUMBER} paris-music-app:latest"
                }
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
