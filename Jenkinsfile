pipeline {
    agent any

    environment {
        SLACK_CHANNEL = '#sending-notifications'  // Change to your Slack channel name
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/aabioumaima/maven-jenkins-skala.git'  // Replace with your GitHub repository URL
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        sh 'mvn clean compile'
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        sh 'mvn test'
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    try {
                        sh 'mvn package'
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    try {
                        // Replace with your actual deployment command
                        sh 'mvn deploy'
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                def message = currentBuild.result == 'SUCCESS' ? 'Build Succeeded' : 'Build Failed'
                slackSend(
                    channel: "${env.SLACK_CHANNEL}", 
                    color: currentBuild.result == 'SUCCESS' ? 'good' : 'danger', 
                    message: "${message}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
                )
            }
        }
    }
}




