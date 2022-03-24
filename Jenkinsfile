pipeline {
    agent { label 'test' }
    stages {
          stage('Build Docker image') {
              when {branch 'khaled' }
                steps {
            echo 'Running build Docker image'
            // tag DockerHubAccountName/RepoName:tag(semver)
            sh 'pwd'
            sh 'whoami'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG .'

        }
    }

          stage('Build Docker image2') {
              when {branch 'master' }
                steps {
            echo 'Running build Docker image'
            // tag DockerHubAccountName/RepoName:tag(semver)
            sh 'pwd'
            sh 'whoami'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG .'

        }
    }

         stage('Build Docker image3') {
              when {branch 'example-solution' }
                steps {
            echo 'Running build Docker image'
            // tag DockerHubAccountName/RepoName:tag(semver)
            sh 'pwd'
            sh 'whoami'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG .'

        }
    }



    }
     }
