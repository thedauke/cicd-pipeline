pipeline {
  agent any
  environment {
    IMAGE_NAME = 'tdads_cicd'
    IMAGE_TAG = '1.0'
  }
  
  stages {
    stage('Git Checkout') {
      steps {
        echo 'Checkout...'
        git(url: 'https://github.com/thedauke/cicd-pipeline.git', branch: 'main', poll: true)
      }
    }

    stage('Build') {
      steps {
        echo 'Building...'
        sh 'chmod +x scripts/build.sh'
        sh 'script ./scripts/build.sh'
      }
    }

    stage('Test') {
      steps {
        echo 'Testing...'
        sh 'chmod +x scripts/test.sh'
        sh 'script ./scripts/test.sh'
      }
    }

    stage('Docker Build') {
      steps {
        echo 'Building Docker image...'
        script {
          sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
        }
      }
    }

    stage('Docker Push') {
      steps {
        echo 'Pushing Docker image...'
        script {
          docker.withRegistry('https://registry.hub.docker.com', '14') {
            sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest"
            sh "docker push ${IMAGE_NAME}:latest"
          }
        }
      }
    }
  }
}
