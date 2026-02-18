pipeline {
    agent any

    environment {
        IMAGE_NAME = 'rashmidevops1/flask-portfolio'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t $IMAGE_NAME:$BUILD_NUMBER ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'Pushing image to DockerHub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred-id', 
                    usernameVariable: 'DOCKER_USER', 
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                        docker push $IMAGE_NAME:$BUILD_NUMBER
                    """
                }
            }
        }

        stage('Deploy to Stage') {
            steps {
                echo 'Deploying application...'
                sh """
                    docker rm -f flask-app || true
                    docker run -d --name flask-app -p 5000:5000 $IMAGE_NAME:$BUILD_NUMBER
                """
            }
        }
    }

    post {
        success {
            echo '✅ Build, Push, and Deploy completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
    }
}
