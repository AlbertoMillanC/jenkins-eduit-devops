pipeline {
    agent any
    environment {
        EC2INSTANCE = 'ec2-user@44.199.211.39'
        APPNAME = 'python-app-demo'
        REGISTRY = 'roxsross12'
        DOCKER_HUB_LOGIN = credentials('docker-hub')
    }

    stages {
        stage('Init') {
            steps {
                echo 'Stage Init'
            }
        }
        stage('Test') {
            steps {
                echo 'Stage Test'
            }
        }
        stage('Build') {
            steps {
                echo 'Stage Build'
                sh 'docker build -t ${APPNAME}:${BUILD_NUMBER} .'
                sh 'docker tag ${APPNAME}:${BUILD_NUMBER} ${REGISTRY}/${APP_NAME}:${BUILD_NUMBER}'
            }
        }
        stage('Push to Registry') {
            steps {
                echo 'Stage Push'
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh 'docker push ${REGISTRY}/${APP_NAME}:${BUILD_NUMBER}'
                
            }
        }
        stage('Deploy') {
            steps {
                echo 'Stage Deploy'
                sh 'touch prueba.txt'
                sshagent(['ssh-ec2']){
                 sh 'scp -o StrictHostKeyChecking=no prueba.txt ${EC2INSTANCE}:/home/ec2-user' 
                }
            }
        }
        stage('Notification') {
            steps {
                echo 'Stage Notify'
            }
        }
    }
}