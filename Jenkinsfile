pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        // If Docker is in PATH, no need for full paths. Otherwise, keep full paths.
        DOCKER_COMPOSE = 'docker-compose'  // assumes docker-compose is in PATH
        DOCKER = 'docker'                  // assumes docker is in PATH
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
            // Optional: stop containers after run
            // bat "\"${DOCKER_COMPOSE}\" -f \"%DOCKER_COMPOSE_FILE%\" down"
        }
    }
}
