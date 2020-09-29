pipeline {
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
                    dockerImage = docker.build('mydocker16121985/devops-capstone:4.0')
                    docker.withRegistry('', 'docker-hub-credentails') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('K8S Deploy')  {
            steps {
                withAWS(credentials: 'aws-creds-eks', region: 'us-west-2') {
                    sh 'aws eks --region us-west-2 update-kubeconfig --name EKSCluster'
                    sh 'kubectl apply -f capstone-deployment.yml'
                }
            }
        }
    }
}