pipeline {
    agent any
    
    stages {
        stage ("Git Clone") {
            steps{ 
                git branch: "main", url: "https://github.com/thanak81/NextCICD-Homework.git" 
            }
        }
        stage ("Build DockerFile"){
            steps{
                sh "docker build -t thanak81/nextcicd-homework-2k25 ."
            }
        }
        stage ("Login to DockerHub"){
            steps{
                script {
                        withCredentials([usernamePassword(credentialsId: "dockerhub-credentials",
                        usernameVariable: "USERNAME", passwordVariable: "PASSWORD"
                )]){
                    sh "docker login --username $username --password $password"
                }
                }
           
            }
        }
        stage ("Push to DockerHub"){
            steps{
                sh "docker push thanak81/nextcicd-homework-2k25"
            }
        }
    }


}