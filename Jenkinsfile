pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "laghusesu/java-image:1.0 ."
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/LaghusesuC/java-app-ec2.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t java-image:1.0 .'
      }
    }

    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'laghusesu',
          passwordVariable: ''
        )]) {
          sh 'echo  | docker login -u laghusesu --password-stdin'
          sh 'docker push java-image:1.0 .'
        }
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          docker stop springboot-container || true
          docker rm springboot-container || true
          docker run -d -p 8080:8080 --name springboot-container java-image:1.0
        '''
      }
    }
  }
}
