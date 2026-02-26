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
    }

    post {
        success {
            echo 'BUILD SUCCESS ✅ Maven build completed'
        }
        failure {
            echo 'BUILD FAILED ❌'
        }
    }
}
