pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')   // ID mình tạo trong Jenkins
        DOCKERHUB_REPO = "thien24/test_aws"                // thay bằng repo Docker Hub của anh
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/thien24/test_aws.git'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    sh """
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                    docker build -t $DOCKERHUB_REPO:latest .
                    docker push $DOCKERHUB_REPO:latest
                    """
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
                    sh """
                    docker rm -f test_aws || true
                    docker pull $DOCKERHUB_REPO:latest
                    docker run -d --name test_aws -p 3000:3000 $DOCKERHUB_REPO:latest
                    """
                }
            }
        }
    }
}
