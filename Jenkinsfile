pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "rakeshvar/dev"
        PROD_IMAGE = "rakeshvar/prod"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'dev', url: 'https://github.com/Rockymca/guvi-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Dev') {
            steps {
                withDockerRegistry([credentialsId: "$DOCKERHUB_CREDENTIALS", url: '']) {
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Push to prod?'
            }
        }

        stage('Push to Prod') {
            steps {
                withDockerRegistry([credentialsId: "$DOCKERHUB_CREDENTIALS", url: '']) {
                    sh 'docker tag $IMAGE_NAME $PROD_IMAGE'
                    sh 'docker push $PROD_IMAGE'
                }
            }
        }
    }
}
