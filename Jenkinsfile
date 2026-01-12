pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        DOCKER_COMPOSE = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker-compose.exe'
        DOCKER = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
        PATH = "C:\\Windows\\System32;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sadiabatool55/Nginx_ELK.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                echo 'Building Docker images...'
                bat "\"${DOCKER_COMPOSE}\" -f \"%DOCKER_COMPOSE_FILE%\" build || exit /b 1"
            }
        }

        stage('Start Containers') {
            steps {
                echo 'Starting containers...'
                bat "\"${DOCKER_COMPOSE}\" -f \"%DOCKER_COMPOSE_FILE%\" up -d || exit /b 1"
            }
        }

        stage('Verify Services') {
            steps {
                echo 'Verifying running containers...'
                bat "\"${DOCKER}\" ps || exit /b 1"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            echo 'Pipeline finished - cleanup or final steps can go here'
            // Optional: bat "\"${DOCKER_COMPOSE}\" -f \"%DOCKER_COMPOSE_FILE%\" down"
        }
    }
}
