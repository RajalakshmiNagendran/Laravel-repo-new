pipeline {
    agent any
    stages {
        stage("Verify Tooling") {
            steps {
                script {
                    echo 'Docker Info:'
                    sh 'docker info'

                    echo 'Docker Version:'
                    sh 'docker version'

                    echo 'Docker Compose Version:'
                    sh 'docker-compose version'
                }
            }
        }

        stage("Clear All Running Docker Containers") {
            steps {
                script {
                    echo 'Clearing running containers...'
                    sh 'docker rm -f $(docker ps -a -q) || true'
                }
            }
        }

        stage('Build and Test') {
            steps {
                dir('/var/lib/jenkins/workspace/laravel') {
                    script {
                        echo 'Running Composer...'
                        sh 'docker-compose run --rm --user $(id -u):$(id -g) composer install'
                        sh 'docker-compose run --rm --user $(id -u):$(id -g) composer update --with-all-dependencies --no-scripts'
                        sh 'docker-compose run --rm --user $(id -u):$(id -g) laravel.test artisan config:clear'
                    }
                }
            }
        }
        
        stage('Deploy stage') {
            steps {
                script {
                    echo 'Deploying containers...'
                    sh 'chmod -R 755 "${WORKSPACE}"'
                    sh 'docker-compose up -d --build'
                }
            }
        }    
    }
}
