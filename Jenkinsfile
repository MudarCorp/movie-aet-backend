pipeline {
    agent any
    tools {
        maven 'Maven' 
    }    
    environment {
        DOCKERHUB_USERNAME = "mudashir"
        APP_NAME = "MOVIE-PROJECT"
        IMAGE_TAG = "1.0.${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/${APP_NAME}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm 
            }
        }
        
        stage('Build Backend') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    dir('C:/Users/user/Desktop/MOVIE-AET-PROJECT/movieist') {
                        sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
