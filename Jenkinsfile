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
                    sh (
                        script: "docker inspect -f '{{.State.Running}}' deployment",
                        returnStdout: true
                    )   
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