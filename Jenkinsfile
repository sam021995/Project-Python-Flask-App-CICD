pipeline {
    agent any

    environment {
        PATH = "/usr/bin:/usr/local/bin:$PATH"   // ensures Jenkins can find docker
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id')
        IMAGE_NAME = 'rashmidevops1/flask-portfolio'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh '/usr/bin/docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'Pushing image to DockerHub...'
                sh '''
                  echo $DOCKERHUB_CREDENTIALS_PSW | /usr/bin/docker login \
                  -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                  /usr/bin/docker push $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Deploy to Stage') {
            steps {
                echo 'Deploying application...'
                sh '''
                  /usr/bin/docker rm -f flask-app || true
                  /usr/bin/docker run -d --name flask-app -p 5000:5000 $IMAGE_NAME:$BUILD_NUMBER
                '''
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
