pipeline {
    agent any

    environment {
        DOCKER_USER = "vignesh00501"
        DOCKER_PASS = credentials('dockerhub-credentials')
    }

    stages {

        stage('Build Docker Image') {
            steps {
                dir('docker-test') {
                    sh 'docker build -t vignesh00501/myapp:v1 .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push vignesh00501/myapp:v1'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                dir('docker-test') {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                }
            }
        }
    }
}
