pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/DO-personal/Aaptatt-hiring-assignment.git'
            }
        }
        stage('Build Maven Project') {
            agent { label 'slave' }
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build and Deploy Docker Containers') {
            agent { label 'slave' }
            steps {
                script {
                    def tomcatImage = docker.build("my-tomcat-image", "-f Docker/Dockerfile.tomcat .")
                    def nginxImage = docker.build("my-nginx-image", "-f Docker/Dockerfile.nginx .")

                    docker.withRegistry('', 'my-dockerhub-credentials') {
                        tomcatImage.push('latest')
                        nginxImage.push('latest')
                    }

                    sh 'docker-compose up --build -d'
                }
            }
        }
    }
}
