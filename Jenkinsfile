pipeline {
    agent any 

    environment {
        K8S_MASTER_PRIVATE_IP = "172.31.36.113"  
        SSH_USER = "user"  
        DEPLOY_YAML_PATH = "deploy.yaml"  

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/darshimackey/kube.git'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        ssh -o StrictHostKeyChecking=no $SSH_USER@$172.31.36.113 '
                            echo "Starting deployment on Kubernetes master"
                            kubectl apply -f $DEPLOY_YAML_PATH
                            kubectl get pods  
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
