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
             environment {
                DOCKER_BUILDKIT = '1'
            }
            steps {
                sh """
                docker buildx create --use || true
                docker buildx build \
                --platform linux/amd64 \
                -t ${DOCKER_IMAGE}:${DOCKER_TAG} \
                -t ${DOCKER_IMAGE}:latest \
                --load .
                """
            }
        }
        stage('login to dockerhub'){
            steps{
                 withCredentials([
                    usernamePassword(
                    credentialsId: 'docker-token',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                    )
                ]) 
                {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    """
                }
            }
        
        }
        stage('push image to dockerhub'){
            steps{
                 sh """
                docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                docker push ${DOCKER_IMAGE}:latest
                """
            }
        
        }
        stage('Deploy on local minikube'){
            environment{
                KUBECONFIG = "/var/lib/jenkins/.kube/config"
            }
            steps{
                 sh """
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                kubectl rollout status deployment/flask-app1
                """
            }
        
        }
    }
     post {
        success {
            echo "✅ App deployed successfully to Minikube"
        }
        failure {
            echo "❌ Deployment failed"
        }
    }    
}