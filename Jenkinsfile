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
}
    stage('SonarCloud Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh '''
            SCAN_VER=5.0.1.3006
            curl -sSL -o sonar.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SCAN_VER}-linux-x64.zip
            rm -rf .sonar-scanner && mkdir -p .sonar-scanner
            unzip -q sonar.zip -d .sonar-scanner
            export PATH="$PWD/.sonar-scanner/sonar-scanner-${SCAN_VER}-linux-x64/bin:$PATH"
            sonar-scanner -Dsonar.login=${SONAR_TOKEN}
          '''
        }
      }
    }

}
