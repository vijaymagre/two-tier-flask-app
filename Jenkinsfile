pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/vijaymagre/two-tier-flask-app.git", branch: "jenkins"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build -t flaskapp ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"vijay",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag flaskapp ${env.dockerHubUser}/flaskapp:latest"
                    sh "docker push ${env.dockerHubUser}/flaskapp:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose up -d --remove-orphans"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    post{
        success{
            script{
                emailext from: 'vijaymagare94@gmail.com',
                to: 'vijaymagare94@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'vijaymagare94@gmail.com',
                to: 'vijaymagare94@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
}
    
}
