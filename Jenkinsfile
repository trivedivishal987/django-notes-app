@Library("shared") _
pipeline {
    agent { label 'vinod' }

    stages {
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage('Code') {
            steps {
                script{
                clone("https://github.com/trivedivishal987/django-notes-app.git","main")
                }
            }
        }

        stage('Build') {
            steps {
                echo 'This is building the code'
                sh "docker build -t note-app:latest ."
                }
        }

        stage('Test') {
            steps {
                echo 'This is testing the code'
            }
        }
        
        stage('Push code in docker-hub') {
            steps {
                echo 'This is pushing the image to docker-hub'
                withCredentials([usernamePassword('credentialsId':"dockerhub-jenkines-access", 
                passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag note-app:latest trivedivishal987/note-app:latest"
                sh "docker push trivedivishal987/note-app:latest"
            }
            }
        }

        stage('Deploy') {
            steps {
                echo 'This is deploying the code'
                sh 'docker compose up -d'
            }
        }
    }
}
