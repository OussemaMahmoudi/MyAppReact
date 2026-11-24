pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Server Image') {
            steps {
                bat 'docker build -t mern-server ./server'
            }
        }

        stage('Build Client Image') {
            steps {
                bat 'docker build -t mern-client ./client'
            }
        }

        stage('Clean Old Containers') {
            steps {
                bat 'docker-compose down'
            }
        }

        stage('Deploy with Compose') {
            steps {
                bat 'docker-compose up -d --build'
            }
        }

        stage('Verify') {
            steps {
                bat 'ping -n 16 127.0.0.1 > nul'
                bat 'docker-compose ps'
                bat 'docker logs server --tail 20'
            }
        }
    }

    post {
        success { echo 'Deployment OK' }
        failure {
            echo 'Pipeline failed'
            bat 'docker-compose logs || exit 0'
        }
    }
}
