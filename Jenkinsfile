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
                sh 'docker build -t mern-server ./server'
            }
        }

        stage('Build Client Image') {
            steps {
                sh 'docker build -t mern-client ./client'
            }
        }

        stage('Clean Old Containers') {
            steps {
                sh 'docker compose down || true'
            }
        }

        stage('Deploy with Compose') {
            steps {
                sh 'docker compose up -d --build'
            }
        }

        stage('Verify') {
            steps {
                sh 'sleep 15'
                sh 'docker compose ps'
                sh 'docker logs server --tail 20'
            }
        }
    }

    post {
        success { echo 'Deployment OK' }
        failure {
            echo 'Pipeline failed'
            sh 'docker compose logs || true'
        }
    }
}
