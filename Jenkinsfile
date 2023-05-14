pipeline {

    agent any
    stages {
        stage('git checkout'){
            steps {
               git 'https://github.com/Prabhakar988/k8proj.git'
            }
          }
        stage('copy files from jenkins to ansible'){
            steps{
                sshagent(['ansible']) {
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38' 
                  sh 'scp /var/jenkins_home/workspace/ansible/* ubuntu@172.31.12.38:/home/ubuntu'  
    
                     }
                    }
                }  

        stage('docker image build'){
            steps{
                sshagent(['ansible']) {
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 cd /home/ubuntu/'
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image build -t $JOB_NAME:v1.$BUILD_ID .'

                 }
               }      
           }
        stage('docker image build'){
            steps{
                sshagent(['ansible']) {
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 cd /home/ubuntu/'
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image tag $JOB_NAME:v1.$BUILD_ID prabhakardevops/$JOB_NAME:v1.$BUILD_ID'
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image tag $JOB_NAME:v1.$BUILD_ID prabhakardevops/$JOB_NAME:latest'
                 }
               }      
           }   
           
}