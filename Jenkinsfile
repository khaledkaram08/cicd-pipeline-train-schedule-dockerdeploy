pipeline {
    agent { label 'node01' }
    stages {
          stage('Build Docker image') {
        steps {
            echo 'Running build Docker image'
            // tag DockerHubAccountName/RepoName:tag(semver)
            sh 'pwd'
            sh 'whoami'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG .'
            
        }
    }
          stage('Push Docker image') {
        steps {
            echo 'Pushing Docker image'
            withCredentials([usernamePassword(credentialsId: 'docker_hub_login', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                
            sh '''
            docker login --username=$USERNAME --password=$PASSWORD
            docker push cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG
            '''
            }

        }
          }



        stage ('Copying YML File') {
            steps{
            sshagent(credentials : ['swarm-staging']) {
            sh 'ssh -o StrictHostKeyChecking=no root@$prod_ip uptime'
            sh 'ssh -v root@$prod_ip'
            sh 'scp -r docker-compose.yml root@$prod_ip:/home/user/workspace/Multi-Tasks_khaled/docker-compose.yml'
        }
    }
}           

            stage('DeployToProduction') {
                when {
                    branch 'khaled'
                }
                steps {
                    input 'Deploy to Production?'
                    milestone(1)
                    withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                        script {
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG\""
                    
                        }
                    sh 'cd /home/user/workspace/Multi-Tasks_khaled'
                    sh 'pwd'
                    sh 'echo $BRANCH_NAME-$BUILD_TAG'
                    sh "sed -i 's!:$BRANCH_NAME-$BUILD_TAG!:$BRANCH_NAME-$BUILD_TAG!g' /home/user/workspace/Multi-Tasks_khaled/docker-compose.yml"
                    sh 'cat /home/user/workspace/Multi-Tasks_khaled/docker-compose.yml'
                    sh 'docker stack rm new-deploy'
                    sh 'docker stack deploy -c /home/user/workspace/Multi-Tasks_khaled/docker-compose.yml new-deploy'
                        
                    }
                }

                
            }

    }
}
