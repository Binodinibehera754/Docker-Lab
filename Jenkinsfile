pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out code from main branch'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t devops-webapp .'
            }
        }

        stage('Test') {
            steps {
                echo 'Starting temporary container and testing application'

                sh '''
                docker run -d \
                --name test-container \
                -p 8080:80 \
                devops-webapp
                '''

                sh 'sleep 5'

                sh '''
                curl --fail http://localhost:8080
                '''

                sh '''
                docker stop test-container
                docker rm test-container
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application'
            }
        }
    }
}
