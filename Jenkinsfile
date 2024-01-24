pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer"
        PATH = "$PATH:${COMPOSER_HOME}/vendor/bin"
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Run Composer install
                    sh 'composer install'
                }
            }
        }

        stage('Build and Run Docker') {
            steps {
                script {
                    // Check if docker-compose.yml has changed
                    def dockerComposeChanged = fileExists('docker-compose.yml.changed')

                    // If changes detected, rebuild and run Docker containers
                    if (dockerComposeChanged) {
                        sh 'docker-compose build'
                        sh 'docker-compose up -d'
                    } else {
                        echo 'No changes in docker-compose.yml. Skipping Docker build and run.'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Print the current PATH for debugging
                    sh 'echo $PATH'
                    
                    // Run PHPUnit tests
                    sh "${COMPOSER_HOME}/vendor/bin/phpunit"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Add your deployment steps here
                    echo 'Deploying...'
                    // Example: Run migrations, clear cache, etc.
                    sh 'php artisan migrate'
                    sh 'php artisan cache:clear'
                }
            }
        }
    }
}
