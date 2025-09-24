pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/thien24/test_aws.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("test_aws:latest")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker stop demo-container || true && docker rm demo-container || true'
                    sh 'docker run -d --name demo-container -p 3000:3000 test_aws:latest'
                }
            }
        }
    }
}
