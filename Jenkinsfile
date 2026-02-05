pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/abdsoha/the-paris-music-books.git'
                    ]]
                ])
            }
        }

        stage('Build with Maven') {
            steps {
                dir('paris-music-app') {
                    sh '''
                        echo "Current directory:"
                        pwd
                        echo "Listing files:"
                        ls -l
                        mvn clean package
                    '''
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
