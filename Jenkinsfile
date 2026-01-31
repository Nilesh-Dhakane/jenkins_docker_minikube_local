pipeline{
    agent any

    environment{
        DOCKER_IMAGE = "nileshdhakane/flask-app1"
        DOCKER_TAG = "latest"
    }

    stages{
        stage('checkout from git'){
            steps{
                script{
                    echo 'Checkout from github'
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/Nilesh-Dhakane/jenkins_docker_minikube_local.git']])
                }
            }
        }
        stage('Build the image'){
            steps{
                script{
                    sh """
                            docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                       """
                }
            }
        }
        stage('login to dockerhub'){
            steps{
                 withCredentials([usernamePassword(
                    credentialsId: 'docker-token',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    """
            }
        }
        
    }
}