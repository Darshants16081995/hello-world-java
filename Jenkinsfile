pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "darshants/sample-app" // Change this to your Docker image name
        DOCKER_USERNAME = "darshants"
        DOCKER_PASSWORD = "darshants@16081995"
        KUBE_USERNAME = "minikubeuser"
        KUBE_PASSWORD = "Darshan@123"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Login to Docker Hub
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    // Build Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                    // Push Docker image to Docker Hub
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Green') {
            steps {
                script {
                    // Deploy to the green environment
                    sh 'kubectl apply -f Deployment_green.yml --username=$KUBE_USERNAME --password=$KUBE_PASSWORD'
                }
            }
        }

        stage('Blue-Green Switch') {
            steps {
                script {
                    // Switch traffic to the green deployment
                    sh 'kubectl apply -f service-switch.yml --username=$KUBE_USERNAME --password=$KUBE_PASSWORD'
                }
            }
        }
    }

    post {
        failure {
            script {
                // Rollback in case of failure
                sh 'kubectl apply -f rollback.yml --username=$KUBE_USERNAME --password=$KUBE_PASSWORD'
            }
        }
    }
}


