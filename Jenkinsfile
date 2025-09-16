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
 sh 'npm test || true'
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
          stage('SonarCloud Analysis') {
    steps {
withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
        withEnv([
            'JAVA_HOME=/opt/java/openjdk',
            'PATH+JAVA=${JAVA_HOME}/bin',
            'SONAR_SCANNER_HOME=/opt/sonar-scanner-5.0.1.3006/bin',
            'PATH+SONAR=${SONAR_SCANNER_HOME}'
        ]) {
            sh '''
                sonar-scanner \
                  -Dsonar.projectKey=JaspreetKaur29_8.2CDevSecOps \
                  -Dsonar.organization=jaspreetkaur29 \
                  -Dsonar.host.url=https://sonarcloud.io \
                  -Dsonar.sources=. \
                  -Dsonar.token=$SONAR_TOKEN
            '''
        }
}
}
}
}
}
