pipeline {
    agent any
    environment {
        KUBECONFIG = '/home/ubuntu/devops-academy-eks-jenkins/infra/kubeconfig_kub-jenkins-infra-alpha-eks'
    }
    stages {
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("ivandbello/pythonapibookub")
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                sh '''
                    cd
                    echo $(kubectl apply -f pythonbookubjen.yml)
                '''
            }
        }
    }
}
