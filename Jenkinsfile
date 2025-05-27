pipeline {
    agent any

    environment {
        IMAGE_NAME = "rakeshvar/dev"
        PROD_IMAGE_NAME = "rakeshvar/prod"
        DOCKER_CREDENTIALS = "dockerhub-creds"  // Add your DockerHub creds to Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning branch: ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        sh 'docker build -t $IMAGE_NAME .'
                    } else if (env.BRANCH_NAME == 'master') {
                        sh 'docker build -t $PROD_IMAGE_NAME .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        if (env.BRANCH_NAME == 'dev') {
                            sh 'docker push $IMAGE_NAME'
                        } else if (env.BRANCH_NAME == 'master') {
                            sh 'docker push $PROD_IMAGE_NAME'
                        }
                    }
                }
            }
        }
    }
}
