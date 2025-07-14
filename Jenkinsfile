pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = '582577265739'    // replace with your AWS Account ID
        AWS_REGION = 'us-east-1'           // replace with your region
        REPO_NAME = 'my-docker-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${REPO_NAME}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_URI
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $REPO_NAME:$IMAGE_TAG ."
            }
        }
        stage('Tag and Push Image') {
            steps {
                sh """
                docker tag $REPO_NAME:$IMAGE_TAG $ECR_URI:$IMAGE_TAG
                docker push $ECR_URI:$IMAGE_TAG
                """
            }
        }
    }
}
