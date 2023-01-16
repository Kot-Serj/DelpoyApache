#!/usr/bin/env groovy

def commitId

pipeline { 
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-login')
        REPOSITORY ='https://index.docker.io'
        NAMESPACE= 'nginx'

    
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
    




    stage ('Deploy to k8s') {
            steps {
                sh "sed -i 's/{{image_tag}}/${commitId}/g' ~/workspace/k8s/wordpress/nginx.yaml"
                sh "sed -i 's/{{namespace}}/${NAMESPACE}/g' ~/workspace/k8s/wordpress/nginx.yaml"
                

                sh "kubectl apply -f ~/workspace/k8s/wordpress/namespace.yaml"
                sh "kubectl apply -f ~/workspace/k8s/wordpress/serv.yaml"
                sh "kubectl apply -f ~/workspace/k8s/wordpress/nginx.yaml"

}
    }

}
}







