pipeline {
    agent any
	stages {
        stage("Verify tooling") {
            steps {
                sh '''
                    docker info
                    docker version
                    docker compose version
                '''
            }
        }
        stage("Clear all running docker containers") {
            steps {
                script {
                    try {
                        sh 'docker rm -f $(docker ps -a -q)'
                    } catch (Exception e) {
                        echo 'No running container to clear up...'
                    }
                }
            }
        }
    		stage('Build') {
                steps {
                    script {
                        // Build Laravel Docker image
                        sh 'docker-compose up --build'
                    }
                }
            }
    		stage('Test') {
                steps {
                    script {
                        // Run Laravel tests
                        sh 'docker-compose run --rm laravel.test php artisan test'
                    }
                }
            }
	}
}
