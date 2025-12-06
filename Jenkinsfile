pipeline {
    agent any
    stages{
        
        stage('CodeScan'){
            steps{
                sh 'trivy fs .  - o result.html'
                sh 'cat result.html'
                
            }
        }
        
        stage('dockerLogin'){
            steps{
                sh 'aws ecr get-login-password --region us-east-2 | \
                docker login --username AWS \
                --password-stdin 440744242159.dkr.ecr.us-east-2.amazonaws.com'
            }
        }
        stage('dockerIamgeBuild'){
            steps{
                sh 'docker build -t jenkins-ci .'
            }
        }
        stage('dockerImageTag'){
            steps{
                sh 'docker tag jenkins-ci:latest \
                440744242159.dkr.ecr.us-east-2.amazonaws.com/jenkins-ci:latest'
            }
        }
        stage('pushImage'){
            steps{
                sh 'docker push 440744242159.dkr.ecr.us-east-2.amazonaws.com/jenkins-ci:latest'

            }
        }
        stage('test'){
            steps{
                sh 'echo success test'
            }
        }
        
    }

}

