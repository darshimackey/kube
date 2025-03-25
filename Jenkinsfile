pipeline {
    agent any  // Run on any available agent

    environment {
        KUBECONFIG = '/home/jenkins/.kube/config'  // Path to the kubeconfig file on the Jenkins server
        KUBECTL_PATH = '/home/jenkins/bin/kubectl'  // Install kubectl in the Jenkins user's bin directory
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
                    // Ensure kubectl is available on the Jenkins server
                    sh '''
                    if [ ! -f $KUBECTL_PATH ]; then
                        echo "kubectl not found, downloading it..."
                        curl -LO https://dl.k8s.io/release/v1.24.0/bin/linux/amd64/kubectl
                        chmod +x kubectl
                        mkdir -p $(dirname $KUBECTL_PATH)  # Create the directory if it doesn't exist
                        mv kubectl $KUBECTL_PATH
                    else
                        echo "kubectl already installed."
                    fi
                    '''

                    // Ensure the kubeconfig file is present at the expected location
                    // If not already configured, copy the Kubeconfig file from your Kubernetes master
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
                    sh '$KUBECTL_PATH --kubeconfig=$KUBECONFIG cluster-info'

                    // Apply the deploy.yaml file using kubectl
                    sh '$KUBECTL_PATH --kubeconfig=$KUBECONFIG apply -f deploy.yaml'

                    // Optionally, you can verify the deployment was successful
                    sh '$KUBECTL_PATH --kubeconfig=$KUBECONFIG get pods'
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
