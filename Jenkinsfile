pipeline {
    agent any

    environment {
        IMAGE_NAME = 'react-nginx-app'
        CONTAINER_NAME = 'react-container'
        HOST_PORT = '8080'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning React application from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building React application using Docker...'

                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Stop Old Container') {
    steps {
        echo 'Removing old container...'
        sh 'docker rm -f react-container || true'
    }
}

       stage('Deploy') {
    steps {
        echo 'Deploying React application using Nginx...'
        sh 'docker run -d --name react-container --restart unless-stopped -p 8081:80 react-nginx-app:${BUILD_NUMBER}'
    }
}

        stage('Verify') {
    steps {
        echo 'Verifying deployment...'
        sh 'sleep 5 && curl -f http://localhost:8081'
    }
}

    post {
        success {
            echo 'React application deployed successfully!'
        }

        failure {
            echo 'Deployment failed. Check Jenkins Console Output.'
        }
    }
}
