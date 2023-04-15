pipeline {
   agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/salmateama20/todo-app-1.git'
            }
        }

//         stage('Install dependencies') {
//             steps {
//                 sh 'npm ci'
//             }
//         }
       stage('Install dependencies') {
             steps {
                dir('todo-app') {
                    sh 'npm install --no-deprecated --omit=optional'
                    sh 'npm run build'
                }
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
