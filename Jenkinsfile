pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo '📂 Cloning the BreakRemainder repository...'
                git branch: 'master', url: 'https://github.com/Pravallika617/Breakremainder.git'
            }
        }

        stage('Docker Build') {
            steps {
                echo '🐳 Building Docker image for BreakRemainder...'
                bat 'docker build -t pravallikas029/breakremainder:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '📦 Pushing image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pass', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        echo === Logging in to Docker Hub ===
                        echo %DOCKER_USER%
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        
                        echo === Pushing image ===
                        docker push %DOCKER_USER%/breakremainder:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '🎉 Image built and pushed successfully to Docker Hub!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the logs.'
        }
    }
}
