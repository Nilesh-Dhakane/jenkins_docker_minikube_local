pipeline{
    agent any

    stages{
        stage('checkout from git'){
            steps{
                script{
                    echo 'Checkout from github'
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/Nilesh-Dhakane/jenkins_docker_minikube_local.git']])
                }
            }
        }
        
    }
}