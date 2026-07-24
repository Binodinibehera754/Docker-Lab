stage('Test') {
    steps {
        echo 'Starting temporary container and testing application'

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
