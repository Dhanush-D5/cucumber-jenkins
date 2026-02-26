pipeline {
    agent any

    tools {
        jdk 'jdk21'
        maven 'maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                bat 'mvn -B clean test'
            }
        }

        stage('Generate Maven Report') {
            steps {
                bat 'mvn -B surefire-report:report-only'
            }
        }
    }

    post {
        always {
            testNG(
                reportFilenamePattern: '**/testng-results.xml',
                escapeTestDescp: true,
                escapeExceptionMsg: true,
                showFailedBuilds: true
            )

            archiveArtifacts artifacts: 'target/**/*.html, target/**/*.json, target/**/*.xml, **/testng-results.xml',
                             fingerprint: true,
                             allowEmptyArchive: true
        }
    }
}
