pipeline {
    agent any 
    
    tools{
        nodejs 'nodejs 10.19.0'  
        }
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git branch: 'master', changelog: false, poll: false, url: 'https://github.com/jyothi62/nodejs-webapp.git'
            }
        }
        
        stage("Install Dependencies"){
            steps{
                sh "npm install"
            }
        }
                stage("Docker Build & Push"){
            steps{
               sh "docker build -t my-nodejs-app ."
               withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                
                sh "docker tag my-nodejs-app ${env.dockerHubUser}/my-nodejs-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-nodejs-app:latest"
                }
            }
        }
        
        stage("Docker Deploy"){
            steps{
                sh "docker run -d --name demo-nodejs -p 8081:8081 jyothi62/my-nodejs-app:latest "
            }
        }
    }
}
