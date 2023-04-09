pipeline{
    agent any
    environment {
        DOCKERHUB_USERNAME = "aruncontainers"
        APP_NAME = "gitops-cd"
        IMAGE_TAG = "${BUILD_NUMBER}" 
        IMAGE_NAME = "$(DOCKERHUB_USERNAME)" + "/" + "$(APP_NAME)"
        REGISTRY_CREDS = "dockerhub"
    }
    stages{
        stage('clean workspace'){
            steps{
                script{
                    clearWs()
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
           
       
    }
}
