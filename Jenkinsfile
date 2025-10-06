@Library("security_stages") _

pipeline {
    environment {
        DOCKERTAG = 'boosef-juiceshop:latest'
        SNYK_CFG_ORG = 'snyk-certification-vinny'
    }
    agent any
    stages {
        stage('Project Build') { // Install any dependencies you need to perform testing
            steps {
                script {
                sh """
                npm install
                """
                }
            }
        }
        stage ("Attempting security stages") {
            steps {
                script {
                withCredentials([string(credentialsId: 'snyk_cli', variable: 'secretText')]) {
                    sh "snyk auth ${secretText}"
                }
                sh """
                snyk test --project-name=juice-shop-deploy
                snyk code test --project-name=juice-shop-deploy
                """
                }
            }
        }
    }
}
