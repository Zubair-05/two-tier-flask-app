pipeline {
    
    agent any;
    
    stages{
        stage("Code Clone"){
            steps{
                git url:"https://github.com/Zubair-05/two-tier-flask-app.git", branch:"master"
            }
        }
        stage("Build"){
            steps{
                echo "Building the docker image"
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Pushing to Docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}" 
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Docker compose se deploy hogya"
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}