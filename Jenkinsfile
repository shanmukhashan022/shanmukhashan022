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
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/shanmukhashan022/jenkinspipeline-dockertest.git'
            }
        }

        stage('Build Docker Image') {
          steps {
              sh 'docker build -t shanmukhashan022/new_jenkins:${BUILD_NUMBER}.'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    withCredentials([string(credentialsId: 'DOCKER', variable: 'passwd')]) {
           sh    'docker login -u shanmukhashan022 -p ${passwd}'
           sh    'docker push shanmukhashan022/new_jenkins1:${BUILD_NUMBER}'
           }
          }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.200:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://10.1.1.200:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 shanmukhashan022/new_jenkins'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.200:8000'
          }
        }

    }
}
