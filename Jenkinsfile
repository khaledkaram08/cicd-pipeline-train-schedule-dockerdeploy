pipeline {
    agent { label 'test' }
    stages {
          stage('Build Docker image') {
        steps {
            echo 'Running build Docker image'
            // tag DockerHubAccountName/RepoName:tag(semver)
            sh 'pwd'
            sh 'whoami'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BUILD_TAG .'
            

        }
    }
          stage('Push Docker image') {
        steps {
            echo 'Pushing Docker image'
            withCredentials([usernamePassword(credentialsId: 'docker_hub_login', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                
            sh '''
            docker login --username=$USERNAME --password=$PASSWORD
            docker push cloudtesttt/docker-image-guru:$BUILD_TAG
            '''
            }

        }
          }        

            stage('DeployToProduction') {
                when {
                    branch 'master'
                }
                steps {
                    input 'Deploy to Production?'
                    milestone(1)
                    withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                        script {
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull cloudtesttt/docker-image-guru:$BUILD_TAG\""
                            
                            try {
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stop train-schedule\""
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker rm train-schedule\""
                        } catch (err) {
                            echo: 'caught error: $err'
                        }
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"cd /home/user/workspace/New-Project_master\""
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stack deploy -c /home/user/workspace/New-Project_master/docker-compose.yml new-deploy\""
                        }
                    }
                }

                
            }

    }
}
