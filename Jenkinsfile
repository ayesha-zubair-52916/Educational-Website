
pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = 'ayeshazubair/educational-website-frontend'
        BACKEND_IMAGE = 'ayeshazubair/educational-website-backend'
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
                bat 'docker build -t %FRONTEND_IMAGE% ./frontend'
                bat 'docker build -t %BACKEND_IMAGE% ./backend'
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_TOKEN'
                )]) {
                    bat 'echo %DH_TOKEN% | docker login -u %DH_USER% --password-stdin'
                    bat 'docker push %FRONTEND_IMAGE%'
                    bat 'docker push %BACKEND_IMAGE%'
                    bat 'docker logout'
                }
            }
        }
    }
}
