pipeline {
    agent { label 'test' }
    stages {
          stage('Build Docker image') {
        steps {
            echo 'Running build Docker image'
            // tag DockerHubAccountName/RepoName:tag(semver)
            sh 'pwd'
            sh 'whoami'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BUILD_NUMBER .'
            

        }
    }
          stage('Push Docker image') {
        steps {
            echo 'Pushing Docker image'
            withCredentials([usernamePassword(credentialsId: 'docker_hub_login', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                
            sh '''
            docker login --username=$USERNAME --password=$PASSWORD
            docker push cloudtesttt/docker-image-guru:$BUILD_NUMBER
            '''
            }

        }
          }


        stage ('Copy') {
            steps{
            sshagent(credentials : ['swarm-staging']) {
            sh 'ssh -o StrictHostKeyChecking=no root@$prod_ip uptime'
            sh 'ssh -v root@$prod_ip'
            sh 'scp docker-compose.yml root@$prod_ip:/home/user/workspace/New-Project_master'
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
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull cloudtesttt/docker-image-guru:$BUILD_NUMBER\""
                            
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stack deploy -c /home/user/workspace/New-Project_master/docker-compose.yml new-deploy\""
                        }
                    }
                }
            }

    }
    } }
