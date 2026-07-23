pipeline {
    agent any

    triggers {
        cron('34 22 * * *')
    }
    tools {
        maven 'Maven'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/favjoyce/calculatorJenkins', branch: 'main'
            }
        }

        stage('Compile') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Coverage') {
            steps {
                bat 'mvn jacoco:report'
                publishHTML (target: [
                    reportDir: 'target/site/jacoco',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Report'
                ])
                bat 'mvn jacoco:check'
            }
        }

        stage('Static Code Analysis') {
            steps {
                bat 'mvn checkstyle:checkstyle'
                publishHTML (target: [
                    reportDir: 'target/reports',
                    reportFiles: 'checkstyle.html',
                    reportName: 'Checkstyle Report'
                ])
                bat 'mvn checkstyle:check'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished.'
        }
        success {
            script {
                emailext (
                    subject: "[SUCCESS] Jenkins: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                    body: '''${SCRIPT, template="groovy-html.template"}''',
                    to: 'anyiafavour15@gmail.com',
                    mimeType: 'text/html'
                )
            }
        }
        failure {
            script {
                emailext (
                    subject: "[FAILED] Jenkins: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                    body: '''${SCRIPT, template="groovy-html.template"}''',
                    to: 'anyiafavour15@gmail.com',
                    mimeType: 'text/html',
                    attachLog: true
                )
            }
        }
    }
}