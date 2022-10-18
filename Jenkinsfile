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
            }
        }
        
        stage('BUILD') {
            steps {
                sh 'echo $REGISTRY_CRED_PSW | docker login -u $REGISTRY_CRED_USR --password-stdin'

                sh 'docker build ./backend -t x-be:$GIT_COMMIT_SHORT'
                sh 'docker tag x-be:$GIT_COMMIT_SHORT ooxyz/x-be:$GIT_COMMIT_SHORT'
                sh 'docker tag x-be:$GIT_COMMIT_SHORT ooxyz/x-be:latest'

                sh 'docker build ./frontend -t x-fe:$GIT_COMMIT_SHORT'
                sh 'docker tag x-fe:$GIT_COMMIT_SHORT ooxyz/x-fe:$GIT_COMMIT_SHORT'
                sh 'docker tag x-fe:$GIT_COMMIT_SHORT ooxyz/x-fe:latest'
            }
        }

        stage('DEPLOY') {
            steps {
                sh 'docker push ooxyz/x-be:$GIT_COMMIT_SHORT'
                sh 'docker push ooxyz/x-be:latest'
                sh 'docker push ooxyz/x-fe:$GIT_COMMIT_SHORT'
                sh 'docker push ooxyz/x-fe:latest'
            }
        }
    }
}