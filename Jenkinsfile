pipeline{
    agent any
    environment {
        DOCKERHUB_USERNAME = "aruncontainers"
        APP_NAME = "gitops-cd"
        IMAGE_TAG = "${BUILD_NUMBER}" 
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = "dockerhub"
    }
    stages{
        stage('clean workspace'){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('checkout scm'){
            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/azdevops1234/argocicd.git',
                    branch: 'main'
                }
            }
        }
           
        stage('build docker image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('push image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    
                    }
                }
            }

        }
        stage('delete docker'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
    }
}
