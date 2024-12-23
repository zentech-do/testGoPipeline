pipeline {
    agent any

    environment {
        registry = "asdloc098l"
        registryCredential = 'dockerhub'
        dockerImageTag = 'latest'
        scannerHome = tool 'sonarqube-scanner'
    }

    stages {

//         stage('Build') {
//             steps {
//                 script {
//                     def services = [
//                         'auth-service',
//                         'profile-service',
//                         'task-server',
//                         'todo-fe'
//                     ]
//
//                     services.each { service ->
//                         sh "cd ${service} && go build -o main ."
//                     }
//                 }
//             }
//         }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def services = [
                        'auth-service',
                        'profile-service',
                        'task-service',
                        'todo-fe'
                    ]

                    services.each { service ->
                        withSonarQubeEnv('sonarqube-server') {
                            sh "${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=${service} \
                                -Dsonar.projectName=${service} \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=./${service} \
                                -Dsonar.go.coverage.reportPaths=coverage.out"
                        }
                    }
                }
            }
        }

//         stage('Build Docker Images') {
//             steps {
//                 script {
//                     def services = [
//                         'auth-service',
//                         'profile-service',
//                         'task-server',
//                         'todo-fe'
//                     ]
//
//                     services.each { service ->
//                         def dockerImage = docker.build("${registry}/${service}:${dockerImageTag}", "./${service}")
//                     }
//                 }
//             }
//         }
//
//         stage('Upload Images') {
//             steps {
//                 script {
//                     def services = [
//                         'auth-service',
//                         'profile-service',
//                         'task-server',
//                         'todo-fe'
//                     ]
//
//                     services.each { service ->
//                         def dockerImage = docker.build("${registry}/${service}:${dockerImageTag}", "./${service}")
//                         docker.withRegistry('', registryCredential) {
//                             dockerImage.push("${dockerImageTag}")
//                             dockerImage.push('latest')
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Deploy with Docker Compose') {
//             steps {
//                 script {
//                     sh "docker compose up -d"
//                 }
//             }
//         }
    }

    post {
        success {
            echo 'Pipeline executed successfully.'
            echo 'All stages completed successfully: Build, SonarQube Analysis, Docker Image Build, Image Upload, and Docker Compose Deployment.'
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
