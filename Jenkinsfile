pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "ankurpatel/nodejs_app"
        registryCredential = 'Docker_Hub_Login'
        
    }

    stages {

        stage('Cloning Git') {
            steps {
              git 'https://github.com/patel0ankur/k8s_cicd_rollingupdate.git'
                }
            }

        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                   }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential ) {
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
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'app/deployment.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
