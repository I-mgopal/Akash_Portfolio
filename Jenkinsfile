pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/I-mgopal/Akash_Portfolio']]])
            }
        }

        stage('Clean Workspace') {
            steps {
                deleteDir()  // Clean the workspace before checkout
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image without cache
                    bat 'docker build --no-cache -t portfolio-local .'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop and remove any running container with the same name, then run the new one
                    bat '''
                    docker stop portfolio-app || exit 0
                    docker rm -f portfolio-app || exit 0
                    docker run -d -p 80:80 --name portfolio-app portfolio-local
                    '''
                }
            }
        }
    }

    post {
        failure {
            echo '❌ Something went wrong. Check Jenkins logs.'
        }
        success {
            echo '✅ Build and deployment completed successfully.'
        }
    }
}
