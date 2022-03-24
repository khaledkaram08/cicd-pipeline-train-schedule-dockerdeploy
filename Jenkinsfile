pipeline {
    agent { label 'test' }
    stages {
          stage('Build Docker image') {
        steps {
            echo 'Running build Docker image'
            // tag DockerHubAccountName/RepoName:tag(semver)
            sh 'pwd'
            sh 'whoami'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG .'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG .'
            sh 'docker build -t cloudtesttt/docker-image-guru:$BRANCH_NAME-$BUILD_TAG .'

        }
    }
       

    }
}
