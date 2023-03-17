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
    }
}