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


    }
}