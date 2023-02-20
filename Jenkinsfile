node { 
    
    stage('Git Checkout'){
        git 'https://github.com/rajesh1218/Kubernetes_Project.git'
    }
    stage('Send docker file to ansible-server'){
        sshagent(['docker-server']) {
            sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 uname -a'
            sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline-ssh-docker/* ec2-user@35.154.191.16:/home/ec2-user'
        }
    }
    stage('Docker Build') {
        sshagent(['ansible-server']) {
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 cd /home/ec2-user/'
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 docker build -t $JOB_NAME:v1.$BUILD_ID .'
        }
    }
    stage('Docker image tagging') {
        sshagent(['ansible-server']) {
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 docker tag $JOB_NAME:v1.$BUILD_ID rajeshjallu/$JOB_NAME:v1.$BUILD_ID'
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 docker tag $JOB_NAME:v1.$BUILD_ID rajeshjallu/$JOB_NAME:latest'
        }
    }
    stage('Docker Image push to docker hub') {
        sshagent(['ansible-server']) {
           withCredentials([string(credentialsId: 'docker_pass', variable: 'dockerhub_passwd')]) {
                sh "docker login -u rajeshjallu -p ${dockerhub_passwd}"
                sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 docker push rajeshjallu/$JOB_NAME:v1.$BUILD_ID'
                sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 docker push rajeshjallu/$JOB_NAME:latest' 
           }
        }
    }   
}
