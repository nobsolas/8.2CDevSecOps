pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/nobsolas/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true' // Let it continue even if tests fail
            }
            post {
                always {
                    emailext(
                        to: 'nobsolas@gmail.com',
                        subject: "Run Tests Stage - ${currentBuild.currentResult}",
                        body: """The 'Run Tests' stage completed with status: ${currentBuild.currentResult}.

Check attached logs for more details.
""",
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
                    emailext(
                        to: 'nobsolas@gmail.com',
                        subject: "Security Scan Stage - ${currentBuild.currentResult}",
                        body: """The 'Security Scan' stage completed with status: ${currentBuild.currentResult}.

Check attached logs for more details.
""",
                        attachLog: true
                    )
                }
            }
        }
    }
}
