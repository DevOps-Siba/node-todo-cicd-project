pipeline {
    agent {
        label 'dev-server'
    }

    stages {
        stage('Clone code') {
            steps {
                git url: 'https://github.com/DevOps-Siba/node-todo-cicd-project.git',
                    branch: 'main'
            }
        }

        stage('Build and Test') {
            steps {
                sh 'whoami'
                sh 'docker build -t node-app .'
            }
        }
        
        stage('Docker push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass',
                    )]) {
                    sh 'docker login -u $dockerHubUser -p $dockerHubPass'
                    sh 'docker image tag node-app:latest $dockerHubUser/node-app:latest'
                    sh 'docker push $dockerHubUser/node-app:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d --build'
            }
        }
    }
}
