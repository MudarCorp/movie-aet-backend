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
                sh 'docker build -t mudashir/my-backend-movie-app:1.0 -f backend/Dockerfile .'
            }
        }

        stage('Push Backend') {
            steps {
                // Push the backend Docker image to your container registry
                sh 'mudashir/my-backend-movie-app:1.0'
            }
        }

        stage('Checkout Frontend') {
            steps {
                // Checkout the frontend code from its Git repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/your-frontend-repo.git']]])
            }
        }

        stage('Build Frontend') {
            steps {
                // Build the React frontend
                sh 'docker build -t mudashir/my-frontend-movie-app:1.0 -f frontend/Dockerfile .'
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
