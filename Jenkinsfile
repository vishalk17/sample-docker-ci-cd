pipeline {
    agent {
        // Use a node with the 'master' label
        label 'master'
    }
    environment {
        // Define your Docker image tag
        docker_image_name = 'vishalk17/nginx'
        docker_image_tag = 'v1.8'
        DOCKERHUB_CREDENTIALS = credentials('docker-credential') // stored cred. in jenkins id, here it is docker-credential
    }

    stages {
        stage('Build image') {
            steps {
                // Change to the directory containing your Dockerfile and build the image
                sh 'pwd && whoami'
                sh "docker build -t ${docker_image_name}:${docker_image_tag} ."
            }
        }

        stage('Login to Docker') {
            steps {
                // Authentication using stored credentials within Jenkins itself
                sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('Push docker image to Docker Hub') {
            steps {
                sh "docker push ${docker_image_name}:${docker_image_tag}"
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
