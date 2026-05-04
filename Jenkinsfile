pipeline {
  agent any

  environment {
    IMAGE_NAME = 'mongodb-crud'
    IMAGE_TAG  = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install dependencies') {
      steps {
        sh 'npm ci'
      }
    }

    stage('Build Docker image') {
      steps {
        sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
      }
    }

    stage('Validate Docker Compose') {
      steps {
        sh 'docker compose config'
      }
    }

    stage('Smoke test app') {
      steps {
        sh '''
          docker compose up -d
          sleep 10
          curl -f http://localhost:3000
        '''
      }
    }
  }

  post {
    always {
      sh 'docker compose down || true'
    }
  }
}
