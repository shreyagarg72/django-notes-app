pipeline{
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/shreyagarg72/django-notes-app.git", branch:"main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image"
                sh " docker build -t my-notes-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing image to Docker hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]){
    sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
    sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
}

                
            }
        }
        stage("Deploy"){
            steps{
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
