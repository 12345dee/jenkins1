pipeline {
    agent any

    environment {
        IMAGE_NAME = "deepak37/jenkins-docker-demo"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning Git Repository..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker Container..."
                // stop if already running
                sh '''
                    docker stop demo-container || true
                    docker rm demo-container || true
                    docker run -d --name demo-container -p 5000:5000 $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build and container started successfully!"
        }
        failure {
            echo "❌ Build failed!"
        }
    }
}
