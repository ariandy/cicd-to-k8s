pipeline {
    agent any
    
    environment{
        REGISTRY_CRED=credentials('dockerhub')
        GIT_COMMIT_SHORT = sh(returnStdout: true, script: '''echo $GIT_COMMIT | head -c 7''')
    }
    
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'hostname'
                sh 'docker version'
                sh 'ls'
                sh 'pwd'
            }
        }
        
        stage('BUILD') {
            steps {
                sh 'echo $REGISTRY_CRED_PSW | docker login -u $REGISTRY_CRED_USR --password-stdin'
                sh 'ls'
                sh 'pwd'
                sh 'docker build ./backend/Dockerfile -t cilist-pipeline-be:$GIT_COMMIT_SHORT'
                sh 'docker tag cilist-pipeline-be:$GIT_COMMIT_SHORT ooxyz/cilist-pipeline-be:$GIT_COMMIT_SHORT'
                sh 'docker push ooxyz/cilist-pipeline-be:$GIT_COMMIT_SHORT'
            }
        }
    }
}