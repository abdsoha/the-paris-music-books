pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Verify Structure') {
            steps {
                sh '''
                echo "Workspace root:"
                pwd
                ls -l

                echo "Checking paris-music-app:"
                ls -l paris-music-app
                '''
            }
        }

        stage('Build with Maven') {
            steps {
                dir('paris-music-app') {
                    sh 'mvn clean package'
                }
            }
        }
    }

    post {
        success {
            echo '✅ BUILD SUCCESSFUL'
        }
        failure {
            echo '❌ BUILD FAILED'
        }
    }
}
