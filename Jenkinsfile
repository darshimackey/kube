pipeline {
    agent any  

    environment {
        KUBECONFIG = '/home/jenkins/.kube/config'  
    }

    stages {
        stage('Clone GitHub Repository') {
            steps {
                
                git 'https://github.com/darshimackey/kube.git' 
            }
        }

        stage('Configure kubectl') {
            steps {
                script {
                    sh 'which kubectl || curl -LO https://dl.k8s.io/release/v1.24.0/bin/linux/amd64/kubectl && chmod +x kubectl && mv kubectl /usr/local/bin/'

                    sh '''
                    if [ ! -f $KUBECONFIG ]; then
                        echo "Kubeconfig file not found, please copy the kubeconfig from your Kubernetes master"
                        exit 1
                    fi
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Verify if kubectl can interact with the Kubernetes master
                    sh 'kubectl --kubeconfig=$KUBECONFIG cluster-info'

                    // Apply the deploy.yaml file using kubectl
                    sh 'kubectl --kubeconfig=$KUBECONFIG apply -f deploy.yaml'

                    // Optionally, you can verify the deployment was successful
                    sh 'kubectl --kubeconfig=$KUBECONFIG get pods'
                }
            }
        }
    }

    post {
        always {
            // Clean up any resources or notify on completion
            echo "Pipeline completed."
        }

        success {
            echo "Deployment successful!"
        }

        failure {
            echo "Deployment failed!"
        }
    }
}
