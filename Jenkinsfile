pipeline {
    agent any
    environment {
        registry = "679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp"
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
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']])
        {
         script {
                sh 'docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp'
                sh 'docker push 679136127575.dkr.ecr.us-east-1.amazonaws.com/nodeapp'
         }
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
