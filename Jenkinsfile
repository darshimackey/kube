pipeline {
    agent any

    environment {
        SSH_CREDENTIALS_ID = 'K8s-ssh' 
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
                            ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} root@$172.31.36.113 'kubectl apply -f '
                        """
                    }
                }
            }
        }
    }
}
