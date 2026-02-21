pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPO = '268539558488.dkr.ecr.ap-south-1.amazonaws.com/devsecops-app'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh '/usr/bin/docker build -t devsecops-app .'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                /usr/bin/docker login --username AWS --password-stdin 268539558488.dkr.ecr.ap-south-1.amazonaws.com
                '''
            }
        }

        stage('Tag & Push Image') {
            steps {
                sh '''
                /usr/bin/docker tag devsecops-app:latest $ECR_REPO:latest
                /usr/bin/docker push $ECR_REPO:latest
                '''
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f app.yaml'
            }
        }
    }
}
