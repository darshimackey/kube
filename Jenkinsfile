pipeline {
    agent any  // This can be any agent where you want to run the pipeline

    environment {
        K8S_MASTER_PRIVATE_IP = "172.31.36.113"  // Kubernetes Master private IP
        SSH_USER = "user"  // SSH username on the Kubernetes master node
        DEPLOY_YAML_PATH = "deploy.yaml"  // Path to the deploy.yaml file in your Git repository
    }

    stages {
        stage('Clone Repository') {
            steps {
                
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // SSH into Kubernetes master and apply the deploy.yaml using kubectl
                    sh """
                        # SSH into the Kubernetes master and apply the deploy.yaml file
                        ssh -o StrictHostKeyChecking=no ${SSH_USER}@${K8S_MASTER_PRIVATE_IP} '
                            echo "Starting deployment on Kubernetes master"
                            kubectl apply -f ${DEPLOY_YAML_PATH}
                            kubectl get pods  # Optionally, check if the pods are running
                        '
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment to Kubernetes master was successful!"
        }

        failure {
            echo "Deployment to Kubernetes master failed!"
        }
    }
}
