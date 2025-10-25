pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'breakreminder-app'
        DOCKERHUB_USERNAME = 'pravallikas029'          // ✅ your Docker Hub username
        DOCKER_CREDENTIALS_ID = 'dockerhub-pass'       // ✅ Jenkins credentials ID for DockerHub
        GIT_CREDENTIALS_ID = 'github-token'            // ✅ Jenkins credentials ID for GitHub PAT
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "📂 Cloning BreakRemainder repository..."
                git branch: 'main', 
                    url: 'https://github.com/Pravallika617/Breakremainder.git',
                    credentialsId: "${GIT_CREDENTIALS_ID}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🛠️ Building Docker image..."
                    sh 'docker build -t $DOCKERHUB_USERNAME/$DOCKER_IMAGE:latest .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "📦 Pushing Docker image to Docker Hub..."
                }
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_USER/$DOCKER_IMAGE:latest
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo "🚀 Deploying to Kubernetes..."
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs!"
        }
    }
}
