pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer"
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Run Composer install
                    sh "composer install"
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
                    // Set up environment variables for PHPUnit
                    def composerBin = "${COMPOSER_HOME}/vendor/bin"
                    env.PATH = "${composerBin}:${env.PATH}"

                    // Run PHPUnit tests
                    sh 'phpunit'
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
