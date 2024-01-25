pipeline {
    agent any

    stages {
        stage('Build and Test') {
            steps {
                script {
                    // Pull the official Composer image from Docker Hub
                    docker.image('composer:latest').pull()

                    // Run tests or other commands inside the Docker container
                    docker.withRun('composer:latest', 'sh -c "composer install && php artisan test"') {
                    }
                }
            }
        }
    }
}
