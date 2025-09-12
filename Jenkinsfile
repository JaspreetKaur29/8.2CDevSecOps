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
 sh 'npm audit || true' // This will show known CVEs in the output
 }
 }
 }
}
    stage('SonarCloud Analysis') {
      environment {
        SONAR_SCANNER_DIR = "${WORKSPACE}/.sonar/sonar-scanner"
        SONAR_SCANNER_ZIP = "${WORKSPACE}/.sonar/sonar-scanner.zip"
        SONAR_SCANNER_URL = "https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip"
        SONAR_PROJECT_KEY = "JaspreetKaur29_8.2CDevSecOps"
        SONAR_ORG        = "jaspreetkaur29"
        SONAR_HOST       = "https://sonarcloud.io"
      }
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh '''
            set -eux

            mkdir -p .sonar

            if [ ! -d "${SONAR_SCANNER_DIR}" ]; then
              curl -L --fail -o "${SONAR_SCANNER_ZIP}" "${SONAR_SCANNER_URL}"
              unzip -q -o "${SONAR_SCANNER_ZIP}" -d .sonar
              mv .sonar/sonar-scanner-* "${SONAR_SCANNER_DIR}"
              chmod +x "${SONAR_SCANNER_DIR}/bin/sonar-scanner"
            fi

            "${SONAR_SCANNER_DIR}/bin/sonar-scanner" \
              -Dsonar.projectKey="${SONAR_PROJECT_KEY}" \
              -Dsonar.organization="${SONAR_ORG}" \
              -Dsonar.host.url="${SONAR_HOST}" \
              -Dsonar.token="${SONAR_TOKEN}"
          '''
        }
      }
    }

