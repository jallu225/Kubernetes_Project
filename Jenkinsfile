node { 
    
    stage('Git Checkout'){
        git 'https://github.com/jallu225/Kubernetes_Project.git'
    }
    stage('Send docker file to ansible-server'){
        sshagent(['docker-server']) {
            sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.135.177 uname -a'
            sh 'scp -o StrictHostKeyChecking=no /var/jenkins_home/jobs/dockerwithk8s/workspace/* ec2-user@3.110.135.177:/home/ec2-user/'
        }
    }
    stage('Docker Build') {
        sshagent(['ansible-server']) {
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.135.177 cd /home/ec2-user/'
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.135.177 docker build -t $JOB_NAME:v1.$BUILD_ID .'
        }
    }
    stage('Docker image tagging') {
        sshagent(['ansible-server']) {
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.135.177 docker tag $JOB_NAME:v1.$BUILD_ID rajeshjallu/$JOB_NAME:v1.$BUILD_ID'
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.135.177 docker tag $JOB_NAME:v1.$BUILD_ID rajeshjallu/$JOB_NAME:latest'
        }
    }
    stage('Docker Image push to docker hub') {
        sshagent(['ansible-server']) {
           withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_passwd')]) {
                sh "docker login -u rajeshjallu -p ${dockerhub_passwd}"
                sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.135.177 docker push rajeshjallu/$JOB_NAME:v1.$BUILD_ID'
                sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.135.177 docker push rajeshjallu/$JOB_NAME:latest' 
           }
        }
    }   
}
