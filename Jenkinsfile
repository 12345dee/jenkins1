pipeline {
    agent any

    environment {
        IMAGE_NAME = "deepak37/jenkins-docker-demo"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                        docker push $IMAGE_NAME:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying container..."
                sh '''
                    docker stop demo-container || true
                    docker rm demo-container || true
                    docker run -d --name demo-container -p 5000:5000 $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build, Push & Deployment completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs!"
        }
    }
}

