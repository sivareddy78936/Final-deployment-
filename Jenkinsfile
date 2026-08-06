pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        AWS_ACCOUNT_ID = "897258608762"

        BACKEND_IMAGE = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/graphic-backend"
        FRONTEND_IMAGE = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/graphic-frontend"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/sivareddy78936/Final-deployment-.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                dir('Backend') {
                    sh 'docker build -t graphic-backend:v1 .'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('Frontend') {
                    sh 'docker build -t graphic-frontend:v1 .'
                }
            }
        }

        stage('Login to Amazon ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | docker login \
                --username AWS \
                --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Push Backend Image') {
            steps {
                sh '''
                docker tag graphic-backend:v1 $BACKEND_IMAGE:v1
                docker push $BACKEND_IMAGE:v1
                '''
            }
        }

        stage('Push Frontend Image') {
            steps {
                sh '''
                docker tag graphic-frontend:v1 $FRONTEND_IMAGE:v1
                docker push $FRONTEND_IMAGE:v1
                '''
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                kubectl rollout restart deployment backend
                kubectl rollout restart deployment frontend

                kubectl rollout status deployment backend
                kubectl rollout status deployment frontend
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully.'
        }

        failure {
            echo 'CI/CD Pipeline failed.'
        }
    }
}
