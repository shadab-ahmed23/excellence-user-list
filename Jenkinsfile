pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'Cloning the code from GitHub' 
                git url:"https://github.com/shadab-ahmed23/excellence-user-list.git",branch:"master"
            }
        }
        stage('Build') {
            steps {
                echo 'Building the code from '
                sh "docker build -t userlist-app ."
            }
        }
        stage('push to dockerHub') {
            steps {
                echo 'push the image to dockerHub'
                withCredentials([usernamePassword(credentialsId:"DockerHub-ID",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag userlist-app shadabahmed23/userlist-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                     sh "docker push shadabahmed23/userlist-app:latest"
                }
            }
        }
       stage("deploye image"){
            steps{
                script{
                echo "deploye the image"
                
                    kubernetesDeploy (configs: 'deployementservice.yaml',kubeconfigId: 'kubernetes')
                
                sh "docker-compose down && docker-compose up -d"
                }
            
            }
            
        }
    }
}
