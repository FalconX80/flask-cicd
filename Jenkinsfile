pipeline {
  agent any

  environment {
    DOCKER_IMAGE = 'priyanshubhatt80/flask-cicd:latest'
  }

  stages {
    stage('Clone Code') {
      steps {
        git branch: 'master', url: 'https://github.com/FalconX80/flask-cicd.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $DOCKER_IMAGE .'
      }
    }

    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
          sh 'docker push $DOCKER_IMAGE'
        }
      }
    }

    stage('Deploy with Ansible') {
      steps {
        // If you're running Jenkins on WSL or a Linux agent
        sh 'ansible-playbook deploy.yml'
      }
    }
  }
}
