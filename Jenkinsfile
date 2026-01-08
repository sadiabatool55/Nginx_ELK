pipeline {
    agent any

    environment {
        // GitHub credentials stored in Jenkins, e.g., id 'github-creds'
        GIT_CREDENTIALS = credentials('github-creds')
        DOCKER_COMPOSE_FILE = "docker-compose.yml"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Cloning GitHub repository..."
                git(
                    url: 'https://github.com/sadiabatool55/Nginx_ELK.git', 
                    credentialsId: "${GIT_CREDENTIALS}"
                )
            }
        }

        stage('Build Docker Images') {
            steps {
                echo "Building Docker images using docker-compose..."
                sh """
                    docker-compose -f ${DOCKER_COMPOSE_FILE} build
                    docker images
                """
            }
        }

        stage('Start Containers') {
            steps {
                echo "Starting containers using docker-compose..."
                sh """
                    docker-compose -f ${DOCKER_COMPOSE_FILE} up -d
                    docker ps -a
                """
            }
        }

        stage('Wait for Healthchecks') {
            steps {
                echo "Waiting for all containers to become healthy..."
                // Loop until all containers are healthy
                sh """
                    set -e
                    for i in {1..20}; do
                        unhealthy=\$(docker ps --filter "health=unhealthy" -q)
                        if [ -z "\$unhealthy" ]; then
                            echo "All containers are healthy!"
                            break
                        else
                            echo "Waiting for containers to become healthy..."
                            sleep 15
                        fi
                    done
                    docker ps
                """
            }
        }

        stage('Show Logs') {
            steps {
                echo "Showing logs for all containers..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} logs --tail=50"
            }
        }

        stage('Stop Containers') {
            steps {
                echo "Stopping and removing containers..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down"
            }
        }
    }

    post {
        always {
            echo "Pipeline finished. Cleaning up Docker resources..."
            sh "docker system prune -f"
        }
    }
}
