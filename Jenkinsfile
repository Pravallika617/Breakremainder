pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo '📂 Cloning the BreakRemainder repository...'
                git branch: 'master', url: 'https://github.com/Pravallika617/Breakremainder.git'
            }
        }

        stage('Build') {
            steps {
                echo '⚙️ Building the BreakRemainder application...'
                bat 'echo Build stage completed.'
            }
        }

        stage('Docker Build') {
            steps {
                echo '🐳 Building Docker image for BreakRemainder...'
                bat '''
                    docker build -t breakremainder:latest .
                '''
            }
        }

        stage('Docker Run') {
            steps {
                echo '🚀 Running Docker Container for BreakRemainder...'
                bat '''
                    docker stop breakremainder || echo "No existing container to stop"
                    docker rm breakremainder || echo "No existing container to remove"
                    docker run -d --name breakremainder -p 8081:5000 breakremainder:latest
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '✅ Deployment successful!'
                echo '🌐 Open http://localhost:8081 in your browser to access BreakRemainder.'
                bat 'docker ps'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline executed successfully ✅'
        }
        failure {
            echo '❌ Pipeline failed — please check the logs for details.'
        }
    }
}
