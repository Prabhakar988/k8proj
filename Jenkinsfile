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
        stage('image tag'){
            steps{
                sshagent(['ansible']) {
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 cd /home/ubuntu/'
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image tag $JOB_NAME:v1.$BUILD_ID prabhakardevops/$JOB_NAME:v1.$BUILD_ID'
                  sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image tag $JOB_NAME:v1.$BUILD_ID prabhakardevops/$JOB_NAME:latest'
                  }
                }      
              }
        stage('push image to dockerhub'){
          steps{
             sshagent(['ansible']) {
             withCredentials([string(credentialsId: 'docker_pwd', variable: 'Docker_pwd')]) {
               sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker login -u prabhakardevops -p {$Docker_pwd}"
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image push $JOB_NAME:v1.$BUILD_ID prabhakardevops/$JOB_NAME:v1.$BUILD_ID'
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image push $JOB_NAME:v1.$BUILD_ID prabhakardevops/$JOB_NAME:latest'
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 docker image rm prabhakardevops/$JOB_NAME:v1.$BUILD_ID prabhakardevops/$JOB_NAME:latest $JOB_NAME:v1.$BUILD_ID'
                 }

              }
           }
         }
        stage('jenkins to k8'){

          steps{
            sshagent(['kube-demo']) {
              sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.1.81'
              sh 'scp /var/jenkins_home/workspace/ansible/* ubuntu@172.31.1.81:/home/ubuntu/' 

              }
            }
          }
        stage('deployment using k8'){
          steps{
            sshagent(['ansible']) {
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 cd /home/ubuntu/'
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 ansible -m ping node'
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.38 ansible-playbook ansible.yml'
             }
          }
        }   
   }


}