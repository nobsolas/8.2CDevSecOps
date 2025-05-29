pipeline {
 agent any
 
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
                script {
                    def testStatus = 'SUCCESS'
                    try {
                        sh 'npm test'
                    } catch (err) {
                        testStatus = 'FAILURE'
                        currentBuild.result = 'UNSTABLE'
                    } finally {
                        emailext(
                            to: 'nobsolas@gmail.com',
                            subject: "Run Tests Stage - ${testStatus}",
                            body: """The 'Run Tests' stage completed with status: ${testStatus}.

Check attached logs for more details.
""",
                            attachLog: true
                        )
                    }
                }
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
                script {
                    def scanStatus = 'SUCCESS'
                    try {
                        sh 'npm audit'
                    } catch (err) {
                        scanStatus = 'FAILURE'
                        currentBuild.result = 'UNSTABLE'
                    } finally {
                        emailext(
                            to: 'your-email@example.com',
                            subject: "Security Scan Stage - ${scanStatus}",
                            body: """The 'Security Scan' stage completed with status: ${scanStatus}.

Check attached logs for more details.
""",
                            attachLog: true
                        )
                    }
                }
            }
        }
    }
}