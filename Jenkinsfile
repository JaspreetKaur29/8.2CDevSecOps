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
    stage('SonarCloud Analysis') {
      environment {
        SONAR_SCANNER_DIR = '.sonar/sonar-scanner'
        SONAR_VERSION = '5.0.1.3006'
      }
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh '''
            set -eux pipefail

            mkdir -p .sonar
ARCH=$(uname -m)
        case "$ARCH" in
          x86_64)   PLATFORM="linux" ;;
          aarch64|arm64) PLATFORM="linux-aarch64" ;;
          *) echo "Unsupported arch: $ARCH"; exit 1 ;;
        esac

            if [ ! -d "${SONAR_SCANNER_DIR}" ]; then
              curl -sSLf -o .sonar/sonar-scanner.zip \"https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SCANNER_VERSION}-${PLATFORM}.zip"
             unzip -q -o .sonar/sonar-scanner.zip -d .sonar
              mv .sonar/sonar-scanner-* "${SONAR_DIR}"
              chmod +x "${SONAR_DIR}/bin/sonar-scanner"
            fi
             export PATH="$PWD/$SCANNER_DIR/bin:$PATH"

            sonar-scanner \
              -Dsonar.projectKey=JaspreetKaur29_8.2CDevSecOps \
              -Dsonar.organization=jaspreetkaur29 \
              -Dsonar.host.url=https://sonarcloud.io \
              -Dsonar.token="${SONAR_TOKEN}"
          '''
        }
      }
    }
}
}
