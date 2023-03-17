pipeline {

    agent any 

    environment {
        DOCKERHUB_USERNAME = "lscampos"
        APP_NAME = "gitops-argocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = "dockerhub"
    }
    
    stages {

        stage ('Cleanup Workspace') {

            steps {
                script {

                    cleanWs()
                }
            }
        }

        stage ('Checkout SCM') {

            steps {
                script {
                    git credentialsId: 'github',
                    url: 'https://github.com/leonardoscampos/gitops_argocd_project.git',
                    branch: 'main'
                }
            }
        }
        
        stage ('build Docker image') {
            steps {
                script {
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
    }
}