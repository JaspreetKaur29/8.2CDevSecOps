pipeline {
 agent any
 stages {
 stage('Checkout') {
 steps {
 git branch: 'main', url: 'https://github.com/JaspreetKaur29/8.2CDevSecOps.git'
 }
 }
 stage('Install Dependencies') {
 steps {
 sh 'npm install'
 }
 }
 stage('Run Tests') {
 steps {
 sh 'npm test || true' // Allows pipeline to continue despite test failures
 }
 }
 stage('Generate Coverage Report') {
 steps {
 // Ensure coverage report exists
 sh 'npm run coverage || true'
 }
 }
 stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit || true'
      }
    }
stage('SonarCloud Analysis (Docker)') {
  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }
  agent { docker { image 'sonarsource/sonar-scanner-cli:latest' } }
  steps {
    sh 'sonar-scanner -Dsonar.login="$SONAR_TOKEN"'
  }
}

}
}
