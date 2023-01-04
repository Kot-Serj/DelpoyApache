#!/usr/bin/env groovy

def commitId

pipeline { 
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-login')
        REPOSITORY ='https://index.docker.io'

    
    }

    stages {

        stage('Checkout') {
            steps {
                script {
                    commitId = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    echo "${commitId}"
                }
            }
        }
        

        stage('Build and push docker image') {
            steps {
                echo 'Building..'
                sh "echo ${DOCKER_CREDENTIALS_PSW} |  docker login ${REPOSITORY} -u ${DOCKER_CREDENTIALS_USR} --password-stdin"
                sh "docker build -f Dockerfile -t kotara2905/websaite1:'${commitId}' ./"
                sh "docker push 'kotara2905/websaite1:'${commitId}"
            }
        }
    }
}
