pipeline {
    agent any

    environment {
        DOCKER_USER = "dockervibh"
        APP_NAME = "vibh-app"
        IMAGE_TAG = "v${env.BUILD_NUMBER}"
        DOCKER_HUB_IMAGE = "${DOCKER_USER}/practice_java:${IMAGE_TAG}"
    }

    tools {
        jdk 'jdk_17'
        maven 'mvn'
    }

    stages {
        stage('Initialize & Cache') {
            steps {
                sh 'mvn dependency:go-offline -B'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn -T 1C package -B'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ${DOCKER_HUB_IMAGE} .'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'trivy image --severity HIGH,CRITICAL --exit-code 1 ${DOCKER_HUB_IMAGE}'
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker_PAT') {
                        docker.image("${DOCKER_HUB_IMAGE}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'docker rmi ${DOCKER_HUB_IMAGE} || true'
        }
    }
}