
pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = 'ayeshazubair/educational-website-frontend'
        BACKEND_IMAGE  = 'ayeshazubair/educational-website-backend'
    }

    stages {

        stage('Pull Code from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ayesha-zubair-52916/Educational-Website.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                bat 'docker build -t %FRONTEND_IMAGE%:latest -t %FRONTEND_IMAGE%:%BUILD_NUMBER% ./frontend'
                bat 'docker build -t %BACKEND_IMAGE%:latest -t %BACKEND_IMAGE%:%BUILD_NUMBER% ./backend'
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-credentials',
                        usernameVariable: 'DH_USER',
                        passwordVariable: 'DH_TOKEN'
                    )
                ]) {
                    bat '@echo off && echo %DH_TOKEN%| docker login -u %DH_USER% --password-stdin'

                    bat 'docker push %FRONTEND_IMAGE%:latest'
                    bat 'docker push %FRONTEND_IMAGE%:%BUILD_NUMBER%'
                    bat 'docker push %BACKEND_IMAGE%:latest'
                    bat 'docker push %BACKEND_IMAGE%:%BUILD_NUMBER%'
                }
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }

        success {
            echo 'SUCCESS: Both Docker images were pushed to Docker Hub.'
        }

        failure {
            echo 'FAILED: Check the Jenkins Console Output for the error.'
        }
    }
}
