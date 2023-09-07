pipeline {
  agent {
    // Use a node with the 'master' label
    label 'master'
  }

  environment {
    // Define your Docker image tag
    docker_image_name = 'vishalk17/nginx'
    docker_image_tag = 'v1.7'
    DOCKERHUB_CREDENTIALS = credentials('docker-credential') // stored cred. in jenkins id, here it is docker-credential
  }

  stages {
    stage('Build image') {
      steps {
        // Change to the directory containing your Dockerfile and build the image
        sh "docker build -t ${docker_image_name}:${docker_image_tag} ."
      }
    }

    stage('Login in docker') {
      steps {
        // authentication using stored credential within jenkins itself
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push docker image to dockerhub') {
      steps {
        sh 'docker push ${docker_image_name}:${docker_image_tag}'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
