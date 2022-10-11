pipeline {
    agent any  
      stages {
        stage('Clone Repo') {
          steps {
            sh 'rm -rf shanmukhashan022'
            sh 'git clone https://github.com/shanmukhashan022/shanmukhashan022.git'
            }
        }

        stage('Build Docker Image') {
            steps {
              sh 'docker build -t shanmukhashan022/new_jenkins1:${BUILD_NUMBER} .'
              }
        }
        stage('Push Docker image') {
            environment {
                DOCKER_HUB_LOGIN = credentials('docker')
            }
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh    'docker push shanmukhashan022/new_jenkins1:${BUILD_NUMBER}'
            }
        }

        stage('Deploy to nginx') {
            steps {  
                sh    'docker run --name nginx:${BUILD_NUMBER} -d -p 8000:80 nginx'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://3.108.61.54:8000'
          }
        }
      }
}
