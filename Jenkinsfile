pipeline{
    agent any;
    environment {
        APP_NAME = "nextcicd"
        RELEASE = "1.0.0"
        DOCKER_USER = "thanak81"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages {
        stage ("Clean Workspace"){
            steps{
                cleanWs()
            }
        }
        stage ("Checkout from SCM"){
            steps{
                git branch: "main", credentialsId: "github", url: "https://github.com/thanak81/NextCICD-Homework.git"
            }
        }
        stage ("Build & Push Docker Image"){
            steps {
                script {
                    docker.withRegistry("",DOCKER_PASS){
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    docker.withRegistry("",DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }
    }
}