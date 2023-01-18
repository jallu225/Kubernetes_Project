node { 
    
    stage('git checkout'){
        git 'https://github.com/rajesh1218/Kubernetes_Project.git'
    }
    stage('send docker file ansible-server'){
        sshagent(['docker-server']) {
            sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 uname -a'
            sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline-ssh-docker/* ec2-user@35.154.191.16:/home/ec2-user/'
        }
    }
    stage('docker build') {
        sshagent(['ansible-server']) {
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 cd /home/ec2-user/'
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 docker build -t $JOB_NAME:v1.$BUILD_ID .'
        }
    }
    stage('docker tagging') {
        sshagent(['ansible-server']) {
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 docker tag $JOB_NAME:v1.$BUILD_ID rajeshjallu/$JOB_NAME:v1.$BUILD_ID'
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 docker tag $JOB_NAME:v1.$BUILD_ID rajeshjallu/$JOB_NAME:latest'
        }
    }
    stage('docker image push to docker hub') {
        sshagent(['ansible-server']) {
           withCredentials([string(credentialsId: 'docker_pass', variable: 'dockerhub_passwd')]) {
                sh "docker login -u rajeshjallu -p ${dockerhub_passwd}"
                sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 docker push rajeshjallu/$JOB_NAME:v1.$BUILD_ID'
                sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 docker push rajeshjallu/$JOB_NAME:latest'
                
           }
        }
    }   
}
