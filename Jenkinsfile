pipeline{
    agent any;
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
    }
}