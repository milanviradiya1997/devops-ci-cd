pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "milanpateldevops/flask-ci-cd"
        DOCKER_TAG = "latest"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/milanviradiya1997/devops-ci-cd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'milanviradiya97',
                    passwordVariable: 'Milan@123'
                )]) {
                    sh 'echo $passwordVariable | docker login -u $usernameVariable --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop flask-app || true
                docker rm flask-app || true
                docker run -d -p 80:5000 --name flask-app $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "CI/CD Pipeline Completed Successfully 🚀"
        }
        failure {
            echo "Pipeline Failed ❌"
        }
    }
}
