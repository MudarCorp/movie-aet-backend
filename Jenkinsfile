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
        REGISTRY_CREDS = 'dockerhub'
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
                    sh "docker build -t mudashir/my-backend-movie-app:1.0:${BUILD_NUMBER} /https://github.com/MudarCorp/movieist/blob/master/Jenkinsfile"
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker push mudashir/my-backend-movie-app:1.0:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}
