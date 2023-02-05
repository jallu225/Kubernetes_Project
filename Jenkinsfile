node { 
    
    stage('Git Checkout'){
        git 'https://github.com/rajesh1218/Kubernetes_Project.git'
    }
    stage('Send docker file to Docker-server'){
        sshagent(['docker-server']) {
            sh "ssh -o StrictHostKeyChecking=no -l ec2-user 15.206.174.243 uname -a"
            sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/deploytok8s/* ec2-user@15.206.174.243: /home/ec2-user/"
       }
    }
}  