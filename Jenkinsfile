pipeline {
    agent any
    tools {
        maven "Maven"
    }
    stages {
        stage("Code Checkout") {
            steps {
                git url: 'https://github.com/souravkar21/mvnwebapp.git'
            }
        }
        stage("Build") {
            steps {
                bat "mvn clean install package"
            }
        }
        stage("Test") {
            steps {
                bat "mvn test"
            }
        }
    }
    post {
        always {
            bat "echo always"
        }
        success {
            bat "echo success"
        }
        failure {
            bat "echo failure"
        }
    }
}