pipeline {
  agent any
  
  environment {
    IMAGE_NAME = 'farjana078/python-flask'
  }
  
  stages {
    stage('Checkout git') {
      steps {
        git branch: 'main', url: 'https://github.com/Farjana78/Python-flask-deploy.git'
      }
    }
    
    stage('Build docker image') {
      steps {
        sh "docker build -t ${IMAGE_NAME}:latest ."
      }
    }
    
    stage('Push to Dockerhub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh """
            echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin
            docker push ${IMAGE_NAME}:latest
            docker logout
          """
        }
      }
    }
  }
}
