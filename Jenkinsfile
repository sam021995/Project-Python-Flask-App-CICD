pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id')
        IMAGE_NAME = 'rashmidevops1/flask-portfolio'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/rashmigmr13-eng/Project-Python-Flask-App-CICD.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh '''
                  echo $DOCKERHUB_CREDENTIALS_PSW | docker login \
                  -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                  docker push $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Deploy to Stage') {
            steps {
                sh 'docker run -d -p 5000:5000 $IMAGE_NAME:$BUILD_NUMBER'
            }
        }
    }

    post {
        success {
            echo '✅ Build, Push & Deploy successful'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
