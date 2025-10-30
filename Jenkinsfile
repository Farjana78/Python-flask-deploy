pipeline {
  agent any
  environment {
    VENV = 'venv'
  }
  stages {

    stage('Checkout git') {
      steps {
        git branch: 'main', url: 'https://github.com/Farjana78/Python-flask-deploy.git'
      }
    }

    stage('Build docker image') {
      steps {
        sh 'docker build -t farjana078/python-flask:latest .'
      }
    }

    stage('Install Python tools') {
      steps {
        sh '''
          sudo apt-get update
          sudo apt-get install -y python3-venv python3-pip
        '''
      }
    }

    stage('Set up the venv') {
      steps {
        sh '''
          python3 -m venv $VENV
          source $VENV/bin/activate
          pip install --upgrade pip
          pip install -r requirements.txt
        '''
      }
    }

    stage('Run the tests') {
      steps {
        sh '''
          source $VENV/bin/activate
          python -m unittest discover -s tests
        '''
      }
    }

    stage('Push to Dockerhub') {
      when {
        branch 'main'
      }
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push farjana078/python-flask:latest
          '''
        }
      }
    }

  }
}
