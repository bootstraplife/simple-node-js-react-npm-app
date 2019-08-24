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
        stage('Build') {
            steps {
                echo 'Building!'
                sh 'docker build -t testimage .'
                sh 'docker run -p 80:80 testimage'
            }
        }
    }
}