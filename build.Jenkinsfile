pipeline {
    agent {
        docker {
            label 'general'
            image '352708296901.dkr.ecr.eu-north-1.amazonaws.com/jenkins_agent:0.1'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Build') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')
                ]) {
                    sh '''
                    docker login --username $USERNAME --password $PASSWORD
                    docker build -t roberta:latest .
                    docker tag roberta:latest alonithuji/roberta:latest
                    docker push alonithuji/roberta:latest
                    '''
                }
            }
            post {
               always {
                 sh '''
                 docker image prune -f -a --filter "until=240h"
                 '''
               }

            }
        }
        stage('Trigger Deploy') {
            steps {
                build job: 'RobertaDeploy', wait: false, parameters: [
                    string(name: 'ROBERTA_IMAGE_URL', value: "alonithuji/roberta:latest")
                ]
            }

        }

    }
}