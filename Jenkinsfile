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
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }
  steps {
    sh '''
      SCAN_DIR="$WORKSPACE/.sonar_scanner"
      mkdir -p "$SCAN_DIR"
      cd "$SCAN_DIR"

      # Download SonarScanner CLI (Linux x64). Adjust version if needed.
      if [ ! -d "sonar-scanner" ]; then
        curl -L -o sonar.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
        unzip -q sonar.zip
        mv sonar-scanner-* sonar-scanner
      fi

      cd "$WORKSPACE"
      "$SCAN_DIR/sonar-scanner/bin/sonar-scanner" \
        -Dsonar.login="$SONAR_TOKEN"
    '''
  }
}
}
}
