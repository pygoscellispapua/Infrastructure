def frontendImage="zuobobrem/frontend"
def backendImage="zuobobrem/backend"
def backendDockerTag=""
def frontendDockerTag=""
def dockerRegistry=""
def registryCredentials="dockerhub"

pipeline {
    agent {
  label 'agent'
}

    environment {
        PIP_BREAK_SYSTEM_PACKAGES = 1
    }

    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }

        stage('Adjust version') {
            steps {
                script{
                    backendDockerTag = params.backendDockerTag.isEmpty() ? "latest" : params.backendDockerTag
                    frontendDockerTag = params.frontendDockerTag.isEmpty() ? "latest" : params.frontendDockerTag
                    
                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
        }
        stage('Clean running containers') {
            steps {
                sh "docker rm -f frontend backend"
            }
        }
        stage('Deploy application') {
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                       docker.withRegistry("$dockerRegistry", "$registryCredentials") {
                            sh "docker-compose up -d"
                        }
                    }
                }
            }
       stage('Deploy application') {
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                       docker.withRegistry("$dockerRegistry", "$registryCredentials") {
                            sh "docker-compose up -d"
                        }
                    }
                }
            }

    post {
        always {
          sh "docker-compose down"
          cleanWs()
        }
    }
}
