node { 
    stage('Git Checkout'){
        git 'https://github.com/rajesh1218/Kubernetes_Project.git'
    }
    stage('Send docker file to ansible-server'){
        sshagent(['docker-server']) {
            sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 3.110.174.129 uname -a'
            sh 'scp -o StrictHostKeyChecking=no /var/jenkins_home/jobs/deploytok8s/workspace/* ec2-user@35.154.191.16:/home/ec2-user'
        } 
}
