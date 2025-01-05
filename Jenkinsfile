pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        echo 'Checkout...'
        git(url: 'https://github.com/thedauke/cicd-pipeline.git', branch: 'main', poll: true)
      }
    }

    stage('Application Build') {
      steps {
        echo 'Building...'
        sh 'chmod +x scripts/build.sh'
        sh 'script ./scripts/build.sh'
      }
    }

    stage('Tests') {
      steps {
        echo 'Testing...'
        sh 'chmod +x scripts/test.sh'
        sh 'script ./scripts/test.sh'
      }
    }

    stage('Docker Image Build') {
      steps {
        echo 'Building Docker image...'
        script {
          sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
        }

      }
    }

    stage('Docker Image Push') {
      steps {
        echo 'Pushing Docker image...'
        script {
          docker.withRegistry('https://registry.hub.docker.com', '14') {
            def app = docker.image("${IMAGE_NAME}:${IMAGE_TAG}")
            app.push("${IMAGE_TAG}")
            app.push('latest')
          }
        }

      }
    }

  }
  environment {
    IMAGE_NAME = 'tdads/cicd'
    IMAGE_TAG = '1.0'
  }
}