pipeline {
    agent any

    stages {
        stage('git') {
            steps {
                git 'https://github.com/karthikkondagari/curd-dd-task-mean.git'
            }
        }
          stage('Build Backend Image') {
            steps {
                sh 'docker build -t karthik008/back:latest ./crud-dd-task-mean-app/backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
               sh 'docker build -t karthik008/front:latest ./crud-dd-task-mean-app//frontend'
            }
        }
          stage('Push Images to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                        usernameVariable: 'karthik008',
                        passwordVariable: 'karthik@123')]) {

                    sh 'echo karthik@123 | docker login -u karthik008 --password-stdin'
                    sh 'docker push karthik008/front:latest'
                    sh 'docker push karthik008/back:latest'
                }
            }
        }
         stage('Deploy to VM') {
            steps {
                
                    sh """
    
                        cd crud-dd-task-mean-app
                        docker-compose down &&
                        docker-compose up -d
                    
                    """
                
            }
        }
    }
}
