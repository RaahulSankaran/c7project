pipeline {
    agent any
    environment {
        registry = "679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp"
        AWS_ACCOUNT_ID="679136127575"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="nodeapp"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/RaahulSankaran/c7project.git']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
            sh 'docker build github.com/RaahulSankaran/c7project.git'
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
        script {
                sh 'sudo docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp'
                sh 'docker tag node:alpine 679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp'
                sh 'docker push 679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp'
         }
        }
      }
   
         
      
    stage('Docker Run') {
     steps{
         script {
                sh 'sudo su'
                sh 'ssh -i raahul-key.pem ubuntu@10.0.2.9'
                sh 'docker run -d -p 8080:8080 --rm --name node 679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp'
            }
      }
    }
    }
}
