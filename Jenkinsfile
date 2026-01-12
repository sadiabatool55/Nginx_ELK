pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        // Corrected paths without extra quotes
        DOCKER_COMPOSE = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker-compose.exe'
        DOCKER = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sadiabatool55/Nginx_ELK.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                // Use double quotes around variables inside bat
                bat "\"${DOCKER_COMPOSE}\" -f \"%DOCKER_COMPOSE_FILE%\" build"
            }
        }

        stage('Start Containers') {
            steps {
                bat "\"${DOCKER_COMPOSE}\" -f \"%DOCKER_COMPOSE_FILE%\" up -d"
            }
        }

        stage('Verify Services') {
            steps {
                bat "\"${DOCKER}\" ps"
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
