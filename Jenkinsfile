pipeline {
     agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true' 
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/salmateama20/todo-app-1.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Run tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    def dockerImage = docker.build("salmateama20/todo-app-1:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker image') {
            steps {
                script {
                    docker.withRegistry('https://github.com/salmateama20/todo-app-1.git') {
                        def dockerImage = docker.image("salmateama20/todo-app-1:${env.BUILD_NUMBER}")
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
