pipeline {
    agent { label 'test' }
    stages {

        stage ('Copy') {
            steps{
            sshagent(credentials : ['swarm-staging']) {
            sh 'ssh -o StrictHostKeyChecking=no root@$prod_ip uptime'
            sh 'ssh -v root@$prod_ip'
            sh 'scp docker-compose.yml root@$prod_ip:/home/user/workspace/New-Project_master'
        }
    }
}

    }
}
