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
                bat 'echo Build stage completed successfully.'
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
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker push %DOCKER_USER%/breakremainder:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚀 Deploying BreakRemainder to Kubernetes...'
                bat '''
                    kubectl delete deployment breakremainder --ignore-not-found
                    kubectl apply -f k8s\\deployment.yaml
                    kubectl apply -f k8s\\service.yaml
                    kubectl get pods
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline executed and deployed successfully!'
        }
        failure {
            echo '❌ Pipeline failed — please check the logs for details.'
        }
    }
}
