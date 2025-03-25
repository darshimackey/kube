pipeline {
    agent any

    environment {
        SSH_CREDENTIALS_ID = 'K8s-ssh'  
        GITHUB_REPO_URL = git 'https://github.com/darshimackey/kube.git'
        K8S_MASTER_PRIVATE_IP = '172.31.36.113'
        DEPLOY_YAML_PATH = 'deploy.yaml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${GITHUB_REPO_URL}"  
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use the SSH credentials to connect to the Kubernetes master and apply the YAML file
                    withCredentials([sshUserPrivateKey(credentialsId: "${SSH_CREDENTIALS_ID}", keyFileVariable: 'SSH_KEY')]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} user@${K8S_MASTER_PRIVATE_IP} 'kubectl apply -f ${DEPLOY_YAML_PATH}'
                        """
                    }
                }
            }
        }
    }
}
