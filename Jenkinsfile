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
                echo 'Testing Docker application'

                sh '''
                docker rm -f test-container || true

                docker run -d \
                --name test-container \
                -p 8081:80 \
                devops-webapp
                '''

                sh 'sleep 5'

                sh '''
                curl --fail http://localhost:8081
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
