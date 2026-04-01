pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-2'
        IMAGE_ECR_REPO = '440744242159.dkr.ecr.us-east-2.amazonaws.com/jenkins-ci'
        ECR_REPO = '440744242159.dkr.ecr.us-east-2.amazonaws.com'
    }

    stages {

        stage('CodeScan') {
            steps {
                sh '''
                    if command -v trivy >/dev/null 2>&1; then
                        trivy fs --format table . > result.txt
                        cat result.txt
                    else
                        echo "Trivy not installed, skipping scan"
                    fi
                '''
            }
        }

        stage('DockerLogin') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('DockerImageBuild') {
            steps {
                sh 'docker build -t jenkins-ci:latest .'
            }
        }

        stage('DockerImageTag') {
            steps {
                sh "docker tag jenkins-ci:latest $IMAGE_ECR_REPO:latest"
                sh "docker tag jenkins-ci:latest $IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
            }
        }

        stage('PushImage') {
            steps {
                sh "docker push $IMAGE_ECR_REPO:latest"
                sh "docker push $IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
            }
        }

        stage('Test') {
            steps {
                sh 'echo success test'
            }
        }
    }
}