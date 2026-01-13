pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/Akshatadd/boardgame-devops.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t boardgame-app .'
      }
    }

    stage('Trivy Scan') {
      steps {
        sh 'trivy image boardgame-app'
      }
    }

    stage('Push to ECR') {
      steps {
        sh 'docker tag boardgame-app:latest <ECR_URI>:latest'
        sh 'docker push <ECR_URI>:latest'
      }
    }

    stage('Deploy Infrastructure') {
      steps {
        sh 'cd terraform && terraform apply -auto-approve'
      }
    }
  }
}

