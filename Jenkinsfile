pipeline {
    agent any
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Git checkout') {
            steps {
                checkout scm
            }
        }
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
                sh 'docker kill deployment'
            }
        }
        stage('Build') {
            steps {
                echo 'Building!'
                sh 'docker build -t testimage --rm .'
                sh 'docker run -p 80:80 -d --name deployment testimage'
            }
        }
    }
}