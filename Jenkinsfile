pipeline {
    agent any
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
}    }
}