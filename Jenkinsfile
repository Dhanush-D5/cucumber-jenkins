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
            junit testResults: 'target/surefire-reports/*.xml',
                  allowEmptyResults: true

            archiveArtifacts artifacts: 'target/**/*.html, target/**/*.json, target/**/*.xml',
                             fingerprint: true
        }
    }
}
