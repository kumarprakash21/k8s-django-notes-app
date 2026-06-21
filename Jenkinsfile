pipeline {
    agent {label "vinod"}

    stages {
        stage('Code') {
            steps {
                echo 'This is closing the code'
                git url: "https://github.com/kumarprakash21/docker-django-aap.git", branch:"main"
                echo " Code is cloned"
            }
        }
        stage("Code Build & Test"){
            steps{
                echo "Code Build Stage"
                sh "docker build -t node-app ."
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubCred",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'This is Deploying the project'
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
