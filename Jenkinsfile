node { 
    
    stage('Git Checkout'){
        git 'https://github.com/rajesh1218/Kubernetes_Project.git'
    }
    stage('Send docker file to ansible-server'){
        sshagent(['docker-server']) {
            sh 'ssh -o StrictHostKeyChecking=no -l ec2-user@15.206.174.243 sudo mkdir abc'
       }
    }
}    