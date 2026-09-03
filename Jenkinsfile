pipeline {
  agent any

  environment {
    PATH = "/usr/local/bin:${env.PATH}"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/daohuuduc2003-byte/8.2CDevSecOps.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'npm test || true'
      }
      post {
        always {
          emailext (
            subject: "Jenkins Build #${env.BUILD_NUMBER} - Run Tests stage: ${currentBuild.currentResult}",
            body: "The Run Tests stage has completed with status: ${currentBuild.currentResult}.\n\nBuild URL: ${env.BUILD_URL}",
            to: 'daohuuduc2003@gmail.com',
            attachLog: true
          )
        }
      }
    }
    stage('Generate Coverage Report') {
      steps {
        sh 'npm run coverage || true'
      }
    }
    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit || true'
      }
      post {
        always {
          emailext (
            subject: "Jenkins Build #${env.BUILD_NUMBER} - Security Scan stage: ${currentBuild.currentResult}",
            body: "The NPM Audit (Security Scan) stage has completed with status: ${currentBuild.currentResult}.\n\nBuild URL: ${env.BUILD_URL}",
            to: 'daohuuduc2003@gmail.com',
            attachLog: true
          )
        }
      }
    }
  }
}
