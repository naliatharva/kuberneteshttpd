pipeline {
    agent any

    environment {
        IMAGE_NAME = "kuberneteshttpd"
        IMAGE_TAG = "latest"
    }

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
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'               
            }
        }
        stage ('Push To Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                    )]) {
                    // Push image in to Docker Hub
                    sh '''                                      
                        docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}
                        docker push ${env.dockerHubUser}/$IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }
        /* stage('Debug Kubernetes') {
            steps {
                sh '''
                whoami
                kubectl config current-context || true
                kubectl cluster-info || true
                kubectl get nodes || true
                '''
            }
        } */
        /* stage('Debug') {
            steps {
                sh 'whoami'
                sh 'id'
                sh 'kubectl get nodes'
            }
        } */
        stage('Deploying container to Kubernetes') {
            steps {
                // Deploying container to Kubernetes
                 withKubeConfig(credentialsId: 'Kubeconfig') {
                    sh '''
                        kubectl apply -f Deployment.yml
                        kubectl apply -f Service.yml
                    '''
                }
            }
        }
    }    
}