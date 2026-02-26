pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tapanshikari/java-app"
        DOCKER_TAG = "latest"
        KUBE_NAMESPACE = "default"
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
                bat "docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                    bat "docker push %DOCKER_IMAGE%:%DOCKER_TAG%"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f deployment.yaml"
                bat "kubectl apply -f service.yaml"
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline SUCCESS'
        }
        failure {
            echo 'CI/CD Pipeline FAILED'
        }
    }
}
