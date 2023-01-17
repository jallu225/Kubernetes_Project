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
           sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 35.154.191.16 docker build -t $JOB_NAME.v1.$BUILD_ID .'
        }
    }
}
