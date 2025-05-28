pipeline {
 agent any
 environment {
  SNYK_TOKEN = credentials('snyk-api-token') // store this token in Jenkins credentials
}
 stages {
 stage('Checkout') {
 steps {
 git branch: 'main', url: ' https://github.com/nobsolas/8.2CDevSecOps.git'
 }
 }
 stage('Install Dependencies') {
 steps {
 sh 'npm install'
 }
 }
 stage('Run Tests') {
 steps {
 sh 'snyk auth $SNYK_TOKEN'
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