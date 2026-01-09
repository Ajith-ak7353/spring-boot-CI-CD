pipeline {
    agent any

    tools {
        maven 'Maven-3.9.6'
    }

    environment {
        DOCKERHUB_USER  = "ajith7353"
        IMAGE_NAME      = "springboot-ci-cd"
        IMAGE_TAG       = "%BUILD_NUMBER%"
        CONTAINER_COUNT = 100
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
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER% ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    ajith7353: 'DOCKER_USER',
                    Ajith@123: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                bat """
                docker push %DOCKERHUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER%
                docker tag %DOCKERHUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER% %DOCKERHUB_USER%/%IMAGE_NAME%:latest
                docker push %DOCKERHUB_USER%/%IMAGE_NAME%:latest
                """
            }
        }

        stage('Deploy Multiple Containers') {
            steps {
                bat """
                FOR /L %%i IN (1,1,%CONTAINER_COUNT%) DO (
                  docker rm -f %IMAGE_NAME%_%%i 2>nul
                  docker run -d --name %IMAGE_NAME%_%%i -p 80%%i:8080 %DOCKERHUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER%
                )
                """
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Pipeline completed successfully on Windows"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
