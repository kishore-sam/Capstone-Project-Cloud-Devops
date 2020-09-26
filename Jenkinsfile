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
                sh 'tidy -q -e **/*.html'
                sh '''docker run --rm -i hadolint/hadolint < Dockerfile'''
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
    }
}