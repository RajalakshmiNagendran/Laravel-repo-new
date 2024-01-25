pipeline {
    agent any
    stages {
        stage("Clean Workspace") {
          steps {
            deleteDir()
          }
        }
        stage("Checkout Code") {
          steps {
            script {
              checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/RajalakshmiNagendran/Laravel-repo-new.git']])
            }
          }
        }
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
                    script {
                        echo 'Running Composer...'
                        sh 'docker-compose run --rm composer install'
                        sh 'docker-compose run --rm composer update --with-all-dependencies --no-scripts'
                    }
                }
            }
        stage('Deploy stage') {
            steps {
                script {
                    echo 'Deploying containers...'
                    sh 'docker-compose up -d --build'
                }
            }
        }    
    }
}
