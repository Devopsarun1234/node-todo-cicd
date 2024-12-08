pipeline {
    agent { label 'arun-bubu' }
    stages {
        stage("Clone Code") {
            steps {
                git url: 'https://github.com/Devopsarun1234/node-todo-cicd.git', branch: 'master'
                echo "Pipeline triggered at: $(date)"
            }
        }
        stage("Build and Test Code") {
            steps {
                sh 'docker build -t node-app .'
            }
        }
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                        credentialsId: 'Dockerhubcreds', 
                        usernameVariable: 'dockerhubuser', 
                        passwordVariable: 'dockerhubpass')]) {   
                    sh '''
                        docker login -u ${dockerhubuser} -p ${dockerhubpass}
                        docker tag node-app:latest ${dockerhubuser}/node-app:latest
                        docker push ${dockerhubuser}/node-app:latest
                    '''
                }
            }
        }
        stage("Deploy") {
            steps {
                sh '''
                    docker-compose down
                    docker-compose up -d --build
                '''
            }
        }
    }
}








