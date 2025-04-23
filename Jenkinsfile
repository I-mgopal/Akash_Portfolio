pipeline {
    agent any

    stages {
        stage('Pull from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/I-mgopal/Akash_Portfolio'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build --no-cache -t portfolio-local .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                docker stop portfolio-app || true
                docker rm portfolio-app || true
                docker run -d -p 8085:80 --name portfolio-app portfolio-local
                '''
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
