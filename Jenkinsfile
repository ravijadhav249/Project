pipeline {
    agent any
    stages {
        stage('Clone Repository'){
            steps {
                git branch: 'master',
                url: 'https://github.com/ravijadhav249/Project.git/'
            }
        }
        stage('Build with Maven'){
            steps {
                sh 'mvn clean package -DskipTests'
                
            }
        }   
        stage('Build docker image'){
            steps {
                sh 'docker build -t todo-application-image:latest .'
                
            }
        }               

        stage('Push docker image'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
                    sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
                    sh 'docker tag todo-application-image:latest ravijadhav249/todo-application:latest'
                    sh 'docker push ravijadhav249/todo-application:latest'
                }
            }
        } 
        stage('Deploy with docker compose'){
            steps {
                sh 'docker compose up -d'
                
            } 
        }
        stage('CLeanUp'){
            steps {
                sh 'rm -rf *'
                
            }
        } 
    }
}
