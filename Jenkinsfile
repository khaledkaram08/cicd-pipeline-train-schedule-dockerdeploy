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



            stage('Pull Docker image') {
                when {
                    branch 'master'
                }
                steps {
                    withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                        script {
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG\""
                    
                        }
 
                    }
                }

                
            }

    }
}
