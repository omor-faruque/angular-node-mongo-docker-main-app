pipeline {
    agent any  // Runs on any available agent

    environment {
        REPO_URL = 'https://github.com/omor-faruque/angular-node-mongo-docker-main-app.git'
        BRANCH = 'main'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}", poll: false
                sh 'git submodule update --init --recursive'  // Ensure submodules are checked out
            }
        }

        stage('Initialize'){
            def dockerHome = tool 'myDocker'
            env.PATH = "${dockerHome}/bin:${env.PATH}"
        }

        stage('Build Backend Image') {
            steps {
                dir('backend') {
                    sh 'docker build -t backend:latest .'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('frontend') {
                    sh 'docker build -t frontend:latest .'
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose down'  // Stops and removes existing containers
                sh 'docker compose up --build'  // Builds and starts containers in detached mode
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
