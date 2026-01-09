pipeline {
    agent any

    tools {
        maven 'Maven-3.9.6'
    }

    environment {
        DOCKERHUB_USER = "ajith7353"
        IMAGE_NAME     = "springboot-ci-cd"
        IMAGE_TAG      = "${BUILD_NUMBER}"
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
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'ajith7353',
                    passwordVariable: 'Ajith@123'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    docker.image("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}").push()
                    docker.image("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}").push("latest")
                }
            }
        }

        stage('Deploy Multiple Containers') {
            steps {
                script {
                    sh '''
                    docker rm -f $(docker ps -aq --filter "name=${IMAGE_NAME}") 2>/dev/null || true

                    for i in $(seq 1 ${CONTAINER_COUNT})
                    do
                      docker run -d \
                        --name ${IMAGE_NAME}_$i \
                        -p $((8000 + i)):8080 \
                        ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    done
                    '''
                }
            }
        }
    }
}

