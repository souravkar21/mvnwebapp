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
        stage("Sonar Analysis") {
            steps {
                withSonarQubeEnv("SonarQube") {
                    bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar"
                }
            }
        }
        stage("Deploy") {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'tomcat-cred', path: '', url: 'http://localhost:8085')], contextPath: 'CalC', war: '**/*.war'
            }
        }
        stage("Upload to Artifactory") {
            steps {
                rtMavenDeployer (
                    id: "admin",
                    serverId: "My_Artifactory",
                    releaseRepo: "CalC",
                    snapshotRepo: "CalC"
                )
                rtMavenRun (
                    pom: "pom.xml",
                    goals: "clean install",
                    deployerId: "admin"
                )
                rtPublishBuildInfo (
                    serverId: "My_Artifactory"
                )
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