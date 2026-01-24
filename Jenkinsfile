pipeline {
    agent {
        label 'slave'
    }

    environment {
        DOCKERHUB_REPO = "ajith7353/springboot-ci-cd"
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
                    mvn clean package
                '''
            }
        }

        stage('Docker Compose Build') {
            steps {
                sh '''
                    docker-compose build
                '''
            }
        }

     stage('Push Image to Docker Hub') {
    steps {
        script {
            withDockerRegistry(
                credentialsId: 'd8b583d2-9b88-424c-b261-a0c8a29cc954',
                url: 'https://index.docker.io/v1/'
            ) {
                sh '''
                    docker-compose push
                '''
            }
        }
    }
}

        stage('Deploy using Docker Compose') {
            steps {
                sh '''
                    docker-compose down
                    docker-compose pull
                    docker-compose up -d
                '''
            }
        }
    }

   post {
    success {
        emailext (
            subject: "✅ SUCCESS: ${JOB_NAME} #${BUILD_NUMBER}",
            body: """

The Jenkins pipeline has completed SUCCESSFULLY.

Job Name   : ${JOB_NAME}
Build No   : ${BUILD_NUMBER}
Status     : SUCCESS
Git Branch : ${GIT_BRANCH}

Docker Image:
${DOCKERHUB_REPO}:latest

Build URL:
${BUILD_URL}

Jenkins CI/CD
""",
            to: "jabaraj626@gmail.com"
        )
    }

    failure {
        emailext (
            subject: "❌ FAILURE: ${JOB_NAME} #${BUILD_NUMBER}",
            body: """

The Jenkins pipeline has FAILED.

Job Name   : ${JOB_NAME}
Build No   : ${BUILD_NUMBER}
Status     : FAILURE

Please check the console logs:
${BUILD_URL}


Jenkins CI/CD
""",
            to: "jabaraj626@gmail.com"
        )
    }
}
}
