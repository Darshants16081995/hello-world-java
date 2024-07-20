pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "darshants/sample-app"
        DOCKER_USERNAME = "darshants"
        DOCKER_PASSWORD = "darshants@16081995"
        KUBE_USERNAME = "minikubeuser"
        KUBE_PASSWORD = "Darshan@123"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh 'docker build -t $DOCKER_IMAGE .'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Green') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/deployment-green.yaml --username=$KUBE_USERNAME --password=$KUBE_PASSWORD'
                }
            }
        }

        stage('Blue-Green Switch') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/service-switch.yaml --username=$KUBE_USERNAME --password=$KUBE_PASSWORD'
                }
            }
        }
    }

    post {
        failure {
            script {
                sh 'kubectl apply -f k8s/rollback.yaml --username=$KUBE_USERNAME --password=$KUBE_PASSWORD'
            }
        }
    }
}

