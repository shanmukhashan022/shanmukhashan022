pipeline {
    agent any
    tools {
        maven "mvn 363"
    }
    stages {
        stage('maven 363') {
          steps {
              script{ 
                  sh "mvn clean package"
              }
          }
        }
              
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

        stage('Deploy to Docker Host') {
            steps {  
                sh    'docker -H tcp://43.205.208.230:2375 stop prodwebapp1 || true'
                sh    'docker -H tcp://43.205.208.230:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 shanmukhashan022/new_jenkins'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://43.205.208.230:8000'
          }
        }

    }
}
