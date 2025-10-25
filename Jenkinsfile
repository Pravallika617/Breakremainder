pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-pass'   // Jenkins credential ID for DockerHub
        DOCKER_IMAGE = 'yourdockerhubusername/break-reminder'
        KUBECONFIG_CREDENTIALS_ID = 'kubeconfig-id' // Jenkins credential for kubeconfig
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/break-reminder.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE:latest .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKER_IMAGE:latest
                        """
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: "${KUBECONFIG_CREDENTIALS_ID}", variable: 'KUBECONFIG_FILE')]) {
                        sh """
                        export KUBECONFIG=$KUBECONFIG_FILE
                        kubectl apply -f k8s-deployment.yaml
                        kubectl get pods
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
