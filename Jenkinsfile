pipeline{
    agent{label "sukuna"}
    stages{
        stage("Code"){
            steps{
                echo "This is cloning the code"
                git url: "https://github.com/parkky21/DEVOPS-CI-CD.git",branch:"main"
                echo "code clonning successfull"
            }
        }
        stage("Build"){
            steps{
                echo "This is building the code"
                sh "docker build -t gaming-vite-app:latest . "

            }
        }
        stage("Test"){
            steps{
                echo "This is testing the code"
            }
        }
        stage("Push to Dockerhub"){
            steps{
                echo "This is pushing the code to dockerhub"
                withCredentials([usernamePassword(
                    credentialsId:"dockerhub-key",
                    usernameVariable:"dockerHubUser",
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag gaming-vite-app:latest ${env.dockerHubUser}/gaming-vite-app:latest"
                sh "docker push ${env.dockerHubUser}/gaming-vite-app:latest"
                }
                echo "Pushed to dockerhub !"
            }
        }
        stage("Deploy"){
            steps{
                echo "This is deploying the code"
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}