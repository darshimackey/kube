pipeline {
    agent any

    environment {
        SSH_CREDENTIALS_ID = 'K8s-ssh' 
        K8S_MASTER_PRIVATE_IP = '172.31.36.113'
        DEPLOY_YAML_PATH = 'deploy.yaml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                 git 'https://github.com/darshimackey/kube.git' 
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
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
