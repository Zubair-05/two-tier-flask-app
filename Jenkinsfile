pipeline {
    
    agent {label "dev"};
    
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
        stage("Testing"){
            steps{
                echo "Testing the application"
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

    post{
        success{
            emailtext(
                subject:"Build Successful",
                body: "Good News: Your build was successful!",
                to: "mullamdz0501@gmail.com"
            )
        }
        failure{
            emailtext(
                subject:"Build Failed",
                body: "Bad News: Your build was Failed!",
                to: "mullamdz0501@gmail.com"
            )
        }
    }
}