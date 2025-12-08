pipeline {
    agent any

    tools {
        maven 'Maven_3.6.3'
        jdk 'JDK17'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/yassinjendoubi/StudentsManagement-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Tests') {
            steps {
                echo 'Tests skipped (no MySQL needed)'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
