pipeline {
    agent { label 'test' }
    stages {

        stage ('Copy') {
            steps{
            sshagent(credentials : ['swarm-staging']) {
            sh 'ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip uptime'
            sh 'ssh -v $USERNAME@$prod_ip'
            sh 'scp docker-compose.yml root@$prod_ip:/home/user/workspace/train-schedule_ayman'
        }
    }
}

    }
}
