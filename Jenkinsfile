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
        stage('update deployment file'){
            steps{
                script{
                   sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                   """
                }
            }

        }
        stage('push deployment to git'){
                steps{
                    script{
                        withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {

                        sh """
                        git config --global user.name "azdevops1234"
                        git config --global user.email "azdevops1234@gmail.com"
                        git add deployment.yaml
                        git commit -m "updated deployment.yaml"
                        git push https://github.com/azdevops1234/argocicd.git HEAD:main         
                        """  
                        }

                    }
                }
        }
    }
}
