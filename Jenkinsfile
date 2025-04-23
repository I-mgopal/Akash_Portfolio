pipeline {
    agent any

    environment {
    DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials-id' // Jenkins credential ID
    DOCKER_IMAGE = 'gopal89/portfolio-ci-cd'             // 👈 Updated image name
}


    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        bat 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                        bat "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    bat 'docker stop portfolio-container || true'
                    bat 'docker rm portfolio-container || true'
                    bat "docker run -d -p 80:80 --name portfolio-container ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Something went wrong. Check Jenkins logs.'
        }
    }
}
