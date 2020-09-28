pipeline {
    environment {
        eksClusterName = 'EKSCluster'
        eksRegion = 'us-west-2'
        dockerHub = 'mydocker16121985'
        dockerImage = 'devops-capstone'
        dockerVersion = '0.1'
    }
    agent any
    stages {
        stage('Lint') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Docker build') {
            steps {
                script {
                    dockerImage = docker.build('${dockerHub}/${dockerImage}:${dockerVersion}')
                    docker.withRegistry('', 'docker-hub-credentails') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('K8S Deploy')  {
            steps {
                withAWS(credentials: 'aws-creds-eks', region: eksRegion) {
                    sh 'aws eks --region=${eksRegion} update-kubeconfig --name ${eksClusterName}'
                    sh 'kubectl apply -f capstone-deployment.yml'
                }
            }
        }
    }
}