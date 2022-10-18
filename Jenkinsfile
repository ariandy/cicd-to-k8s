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
                sh 'aws --version'
                sh 'helm version'
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

        stage('TEST') {
            steps {
                sh 'docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ooxyz/x-be:$GIT_COMMIT_SHORT'
                sh 'docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ooxyz/x-fe:$GIT_COMMIT_SHORT'
            }
        }

        stage('DEPLOY') {
            steps {
                sh 'docker push ooxyz/x-be:$GIT_COMMIT_SHORT'
                sh 'docker push ooxyz/x-be:latest'
                sh 'docker push ooxyz/x-fe:$GIT_COMMIT_SHORT'
                sh 'docker push ooxyz/x-fe:latest'

                withAWS(credentials: 'jenkins-aws-key', region: 'us-west-2') {
                    sh 'aws iam get-user'
                }
            }
        }
    }
}