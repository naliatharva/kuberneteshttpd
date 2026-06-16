pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Clean workspace before cloning (optional)
                deleteDir()

                // Clone the Git repository
                git branch: 'main',
                    url: 'https://github.com/naliatharva/kuberneteshttpd.git'
            }
        }
        stage('Build Docker Image') {
            steps {                
                // Build the Docker image
                sh 'docker build -t kuberneteshttpd:latest .'               
            }
        }
        stage('Deploying container to Kubernetes') {
            steps {
                // Deploying container to Kubernetes
                kubernetesDeploy(
                    configs: 'Deployment.yml,Service.yml'
                )
            }
        }
    }
}