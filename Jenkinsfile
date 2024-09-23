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
        stage ("Checkout from Our K8S manifest repository"){
            steps{
                git branch : "main" , credentialsId: "github" , url : "https://github.com/thanak81/nextcicd-k8s_manifest.git"
            }
        }
           stage ("Update the Deployment Tags"){
            steps {
                script {
                sh """
                    cat k8s/deployment.yaml
                    sed -i 's|image: ${DOCKER_USER}/${APP_NAME}:.*|image: ${DOCKER_USER}/${APP_NAME}:${IMAGE_TAG}|' k8s/deployment.yaml
                    cat k8s/deployment.yaml
                """
                }
              
            }
        }
    stage ("Push the changed deployment file to GIT"){
            steps{
                script {
                sh """
                    git config --global user.name "thanak81"
                    git config --global user.email "thanakmech@gmail.com"
                    git add k8s/deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                // withCredentials([gitUsernamePassword[credentialsId: "github", gitToolName: "Default"]]){
                //     sh "git push https://github.com/thanak81/nextcicd-k8s_manifest.git main"
                // }
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh "git push https://$USERNAME:$PASSWORD@github.com/thanak81/nextcicd-k8s_manifest.git main"
                }
                }
               
            }
        }
    }
}