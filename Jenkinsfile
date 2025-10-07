@Library("security_stages") _

pipeline {
    environment {
        DOCKERTAG = 'boosef-juiceshop:latest'
        PROJECT_NAME = 'juice-shop-goof'
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
                withCredentials([string(credentialsId: 'snykservicetoken', variable: 'secretText')]) {
                    sh "snyk auth ${secretText}"
                }
                sh """
                snyk test --project-name=${PROJECT_NAME}
                snyk code test --project-name=${PROJECT_NAME} --all-projects
                """
                }
            }
        }
    }
}
