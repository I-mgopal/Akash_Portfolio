pipeline { 
    agent any

    options {
        skipDefaultCheckout(true) // Avoid automatic checkout
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir() // Clean out old files
            }
        }

        stage('Clone Repository') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/I-mgopal/Akash_Portfolio']]])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build --no-cache -t portfolio-local .'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    bat '''
                    docker stop portfolio-app || exit 0
                    docker rm portfolio-app || exit 0
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
