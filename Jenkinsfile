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

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Tests') {
            steps {
                echo 'Tests skipped (no database needed)'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t yassinjendoubi/spring-students:latest -f Dockerfile .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'docker-hub-credentials',
                            usernameVariable: 'DOCKER_USERNAME',
                                 passwordVariable: 'DOCKER_PASSWORD'
                        )
                    ]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                        sh 'docker push yassinjendoubi/spring-students:latest'
                    }
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}

