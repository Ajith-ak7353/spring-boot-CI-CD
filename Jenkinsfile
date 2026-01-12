pipeline {
    agent any

    tools {
        maven 'Maven 3.9.6'
    }

    environment {
        IMAGE_NAME      = "springboot-ci-cd"
        CONTAINER_COUNT = 1
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh '''
                mvn -version
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Deploy Multiple Containers') {
            steps {
                sh '''
                docker rm -f $(docker ps -aq --filter "name=${IMAGE_NAME}_") 2>/dev/null || true

                for i in $(seq 1 ${CONTAINER_COUNT})
                do
                  docker run -d \
                    --name ${IMAGE_NAME}_$i \
                    -p 9090:9090 \
                    ${IMAGE_NAME}:${BUILD_NUMBER}
                done
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully on Ubuntu agent-vikram"
        }
        failure {
            echo "❌ Pipeline failed on agent-vikram"
        }
    }
}
