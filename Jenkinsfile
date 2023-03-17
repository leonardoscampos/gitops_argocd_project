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
        stage ('Docker Push Image') {
            steps {
                script {
                    docker.withRegistry('',REGISTRY_CREDS) {
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage ('Delete Docker images') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage ('Update kubernetes deployment file') {
            steps {
                script {
                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }
        }
    }
}