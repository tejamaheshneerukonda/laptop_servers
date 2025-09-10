pipeline {
  agent any
  environment {
    IMAGE = "hp-server/${JOB_NAME}:${BUILD_NUMBER}"
    CONTAINER = "app-${JOB_NAME}"
    HOST_PORT = "8083"       // HP server port to expose
    CONTAINER_PORT = "8000"  // Flask app port
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build Docker image') {
      steps {
        sh 'docker build -t $IMAGE .'
      }
    }
    stage('Deploy container') {
      steps {
        sh '''
          docker rm -f $CONTAINER || true
          docker run -d --name $CONTAINER -p ${HOST_PORT}:${CONTAINER_PORT} $IMAGE
        '''
      }
    }
  }
  post {
    success {
      echo "Live at: http://10.0.0.150:${HOST_PORT}"
    }
  }
}
