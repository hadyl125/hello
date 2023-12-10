pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/hadyl125/hello.git']]])
            }
        }
        stage('Build Python App') {
            steps {
                script {
                    sh 'python -m pip install -r requirements.txt' // If you have dependencies
                    sh 'python hello.py'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t hadyl125/hello.py .'
                }
            }
        }
        stage('Push Image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u hadyl125-p ${dockerhubpwd}'
                    }
                    sh 'docker push hadyl125/hello.py'
                }
            }
        }
        // Include other stages like 'Deploy to k8s' if needed
    }
}
