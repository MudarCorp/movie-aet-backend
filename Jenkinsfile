pipeline {
    agent any
        tools {
        maven 'Maven'
    }    
    environment {
        DOCKERHUB_USERNAME = "mudashir"
        APP_NAME = "MOVIE-PROJECT"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/${APP_NAME}"
        REGISTRY_CREDS = 'docker-hub'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Backend') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t mudashir/my-backend-movie-app:1.0:${BUILD_NUMBER} ."
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    }
                    sh "docker push mudashir/my-backend-movie-app:1.0:${BUILD_NUMBER}"
                    
                    // Clean up Docker login credentials
                    sh "docker logout your-docker-registry-url"
                }
            }
        }
    }
}
