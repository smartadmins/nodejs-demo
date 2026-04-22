pipeline {
    agent any
    
    triggers {
    githubPush()
    }
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-ksudhy')
        IMAGE_NAME = "ksudhy/nodeapp"
    }

    stages {

        stage('SCM Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/smartadmins/nodejs-demo.git']])
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('DockerHub Login') {
            steps {
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login \
                -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}

