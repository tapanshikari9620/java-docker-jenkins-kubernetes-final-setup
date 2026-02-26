pipeline {
    agent any

    environment {
        IMAGE_NAME = "tapanshikari/rest-api"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'develop',
                    url: 'https://github.com/tapanshikari9620/java-docker-jenkins-kubernetes-final-setup.git'
            }
        }

        stage('Build Maven') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t rest-api:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Tag Image') {
            steps {
                bat 'docker tag rest-api:v1 %IMAGE_NAME%:%IMAGE_TAG%'
            }
        }

        stage('Push Image') {
            steps {
                bat 'docker push %IMAGE_NAME%:%IMAGE_TAG%'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS ✅ Maven + Docker build + DockerHub push completed'
        }
        failure {
            echo 'BUILD FAILED ❌'
        }
    }
}
