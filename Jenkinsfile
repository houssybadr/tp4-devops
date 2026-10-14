pipeline {
    agent any
    environment {
        IMAGE_NAME = 'dareeed/tp4'
        DOCKER_HOST = 'tcp://127.0.0.1:2375'
    }
    stages {
        stage('Cloning Git') {
            steps {
                checkout scm
            }
        }
        stage('Building image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }
        stage('Test image') {
            steps {
                bat 'docker run --rm %IMAGE_NAME%:%BUILD_NUMBER% sh -c "grep -q BDCC /usr/share/nginx/html/index.html"'
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-dareeed') {
                        bat 'docker push %IMAGE_NAME%:%BUILD_NUMBER%'
                    }
                }
            }
        }
    }
}
