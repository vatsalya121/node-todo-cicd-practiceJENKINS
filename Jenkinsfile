pipeline {
    agent { label 'dev-server' }
    stages{
        stage("CODE CLONE"){
            steps{
                git url: "https://github.com/vatsalya121/node-todo-cicd-practiceJENKINS.git", branch: "master"
            }
        }
        stage("CODE BUILD AND TEST"){
            steps{
                sh "docker build -t node-app ."
            }
        }
        stage("Push to docker hub"){
            steps{
                script {
                    withCredentials([usernamePassword(credentialsId:"dockerHubCred", usernameVariable:"dockerHubUser", passwordVariable:"DockerHubPass")]) {
                        sh "echo '$dockerHubPass' | docker login -u '$dockerHubUser' --password-stdin"
                        sh "docker image tag node-app:latest $dockerHubUser/node-app:latest"
                        sh "docker push $dockerHubUser/node-app:latest"
                    }
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
