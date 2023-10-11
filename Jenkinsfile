pipeline {
    agent any

    stages {
        stage('Checkout Backend') {
            steps {
                // Checkout the backend code from its Git repository
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                // Build the Java Spring Boot backend
                dir('https://github.com/MudarCorp/movieist.git') {
                    sh 'docker build -t mudashir/my-backend-movie-app:1.0 -f Dockerfile .'
                }
            }
        }

        stage('Push Backend') {
            steps {
                // Push the backend Docker image to your container registry
                sh 'docker push mudashir/my-backend-movie-app:1.0'
            }
        }

        stage('Checkout Frontend') {
            steps {
                // Checkout the frontend code from its Git repository
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MudarCorp/movie-gold-v1.git']]])
            }
        }

        stage('Build Frontend') {
            steps {
                // Build the React frontend
                dir('https://github.com/MudarCorp/movie-gold-v1.git') {
                    sh 'docker build -t mudashir/my-frontend-movie-app:1.0 -f Dockerfile .'
                }
            }
        }

        stage('Push Frontend') {
            steps {
                // Push the frontend Docker image to your container registry
                sh 'docker push mudashir/my-frontend-movie-app:1.0'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply Kubernetes YAML files for deployment
                sh 'kubectl apply -f Backend Deployment.yaml -f backend-service.yaml -f Frontend Deployment.yaml -f frontend-service.yaml'
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
