pipeline{
    agent { label "dev"};
    stages{
        stage("clone"){
            steps{
                git url: "https://github.com/iamyasirhussain/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage("Test"){
            steps{
                echo "testing"
            }
        }
        stage("Push to Docker hub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerHubCreds",
                passwordVariable:"dockerHubPass",
                usernameVariable:"dockerHubUser"
                )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
            }
            
        }
        
    }
}
