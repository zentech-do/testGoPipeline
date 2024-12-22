pipeline {
    agent any

    environment {
        registry = "asdloc098l"
        registryCredential = 'dockerhub-nhom17'
        dockerImageTag = 'latest'
        //scannerHome = tool 'sonarqube'
    }

    stages {


         stage('Deploy to Minikube') {
                    steps {
                        script {
                            // Kiểm tra kết nối tới Minikube và deploy ứng dụng
                            bat """
                            kubectl apply -f k8s/infrastructure/namespace.yaml
                            """
                        }
                    }
                }
          }

    post {
        success {
            echo 'Pipeline executed successfully.'
            echo 'All stages completed successfully: Build, Docker Image Build, Image Upload, and Docker Compose Deployment.'
        }
        failure {
            echo 'Pipeline failed during execution.'
            echo 'Please check the logs for details.'
        }
        always {
            echo 'Cleaning up after execution...'
            // Add any clean-up steps here if necessary
        }
    }
}
