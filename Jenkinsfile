pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-username/devops-sample-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("your-aws-account-id.dkr.ecr.your-region.amazonaws.com/sample-app")
                }
            }
        }
        stage('Push to ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds-id']]) {
                    script {
                        sh '''
                          aws ecr get-login-password --region your-region | docker login --username AWS --password-stdin your-aws-account-id.dkr.ecr.your-region.amazonaws.com
                          docker push your-aws-account-id.dkr.ecr.your-region.amazonaws.com/sample-app
                        '''
                    }
                }
            }
        }
    }
}
