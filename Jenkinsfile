pipeline {
    agent any
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Check if container is running') {
            when {
                expression {
                    return sh (
                        script: "docker inspect -f '{{.State.Running}}' deployment",
                        returnStatus: true
                    ) == 0
                }
            }
            steps {
                sh 'docker rm -f deployment'
            }
        }
        stage('Build') {
            steps {
                echo 'Building!'
                sh 'docker build -t testimage --rm .'
               // sh 'docker image prune -f'
                sh 'docker run -p 80:80 -d --name deployment testimage'
            }
        }
    }
}