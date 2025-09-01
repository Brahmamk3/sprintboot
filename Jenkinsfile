pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        DOCKER_IMAGE       = "brahmamk015/kubernetes-repo"
        DOCKER_CREDENTIALS = "veera-docker"
        AWS_CREDS          = "aws-eks-creds"
        AWS_REGION         = "eu-west-2"
        EKS_CLUSTER        = "team4-eks-cluster"  // Replace with your cluster name
        KUBE_NAMESPACE     = "veera-namespace"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Brahmamk3/sprintboot.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                    docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDENTIALS,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: "${AWS_CREDS}"]]) {
                    sh '''
                        aws configure set default.region "$AWS_REGION"
                        aws eks update-kubeconfig --region "$AWS_REGION" --name "$EKS_CLUSTER"

                        kubectl get ns "$KUBE_NAMESPACE" >/dev/null 2>&1 || kubectl create ns "$KUBE_NAMESPACE"

                        kubectl apply -n "$KUBE_NAMESPACE" -f k8s/deployment.yaml
                        kubectl apply -n "$KUBE_NAMESPACE" -f k8s/service.yaml
                    '''
                }
            }
        }
    }
}
