pipeline{
    agent any
    stages{
        stage("Code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/surajsingz/Django-Note-App.git", branch:"main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build -t cicd-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerpwd", usernameVariable:"dockeruser")])
                {
                    sh "docker tag cicd-note-app ${env.dockeruser}/cicd-note-app:latest"
                    sh "docker login -u ${env.dockeruser} -p ${env.dockerpwd}"
                    sh "docker push ${env.dockeruser}/cicd-note-app:latest"
                }
        }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
               // sh "docker run -d -p 8000:8000 surajsingz/cicd-note-app:latest"
            }
        }
    }
}