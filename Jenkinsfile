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
        stage('Build and Test') {
            steps {
                // Set the project path using dir
                dir('/var/lib/jenkins/workspace/Laravel-new-ci-cd') {
                    // Run Docker Compose command
                    script {
                        sh 'docker-compose run --rm composer'
                    }
                }
            }
        }
	}
}
