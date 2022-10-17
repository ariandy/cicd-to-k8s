pipeline {
    agent any
    
    environment{
        REGISTRY_CRED=credentials('dockerhub')
    }
    
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'hostname'
                sh 'docker version'
            }
        }
        
        stage('BUILD') {
            steps {
                sh 'echo $REGISTRY_CRED_PSW | docker login -u $REGISTRY_CRED_USR --password-stdin'
                docker build -f Dockerfile.be -t ooxyz/cilist:latest-be
                docker build -f Dockerfile.fe -t ooxyz/cilist:latest-fe
            }
        }
    }
}