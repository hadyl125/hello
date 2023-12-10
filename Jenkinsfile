pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/your-github-repository-url']]])
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
                    sh 'docker build -t your-dockerhub-username/your-app-name .'
                }
            }
        }
        stage('Push Image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u your-dockerhub-username -p ${dockerhubpwd}'
                    }
                    sh 'docker push your-dockerhub-username/your-app-name'
                }
            }
        }
        // Include other stages like 'Deploy to k8s' if needed
    }
}
