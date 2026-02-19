pipeline {
    agent any

    tools {
        // Must match the name in Manage Jenkins > Tools
        maven 'Maven3'
    }

    environment {
        // Senior Practice: Centralize names to avoid "overlapping" conflicts
        IMAGE_NAME = "paris-music-app"
        CONTAINER_NAME = "paris-music"
    }

    stages {
        stage('Checkout') {
            steps {
                // Inherits Git settings from SCM
                checkout scm 
            }
        }

        stage('Build Artifact') {
            steps {
                // Use 'dir' because pom.xml is in a subfolder
                dir('paris-music-app') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Docker Policy & Build') {
            steps {
                dir('paris-music-app') {
                    // Traceability: Tag with Build Number
                    sh "docker build -t ${IMAGE_NAME}:${env.BUILD_NUMBER} ."
                    sh "docker tag ${IMAGE_NAME}:${env.BUILD_NUMBER} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('SRE Deployment & Health Check') {
            steps {
                sh """
                # 1. Clean old state to prevent "Conflict: name already in use"
                docker rm -f ${CONTAINER_NAME} || true
                
                # 2. Deploy: Mapping 9090 (Host) to 8080 (Container Default)
                # If your app specifically uses 9090 internally, change 8080 to 9090
                docker run -d -p 9090:8080 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest
                
                # 3. Verification: Wait for the JVM to initialize
                echo "Monitoring startup for 20 seconds..."
                sleep 20
                
                # 4. Check if container is 'running'
                if [ \"\$(docker inspect -f '{{.State.Running}}' ${CONTAINER_NAME})\" == \"false\" ]; then
                    echo \"CRITICAL ERROR: Container exited! Dumping logs for senior-level debugging:\"
                    docker logs ${CONTAINER_NAME}
                    exit 1
                fi
                echo \"SUCCESS: Deployment is healthy and reachable at http://localhost:9090\"
                """
            }
        }
    }

    post {
        always {
            // Reclaim disk space on the VM
            cleanWs()
            echo "Pipeline finished."
        }
    }
}
