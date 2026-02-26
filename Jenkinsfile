pipeline {
    agent any

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
    }

    post {
        success {
            echo 'BUILD SUCCESS ✅ Maven + Docker build completed'
        }
        failure {
            echo 'BUILD FAILED ❌'
        }
    }
}
