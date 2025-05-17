pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '396913735013'
        AWS_REGION = 'ap-south-1'
        ECR_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/devops-sample-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/VHarinatha/devops-sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${ECR_REPO}")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_sec_keys']]) {
                    script {
                        sh """
                          aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO}
                          docker push ${ECR_REPO}
                        """
                    }
                }
            }
        }
    }
}
