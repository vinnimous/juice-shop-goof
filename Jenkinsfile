@Library("security_stages") _

pipeline {
    environment {
        DOCKERTAG = 'boosef-juiceshop:latest'
        SNYK_CFG_ORG = 'snyk-certification-vinny'
    }
    agent any
    stages {
        // stage('Project Build') { // Install any dependencies you need to perform testing
        //     steps {
        //         script {
        //         sh """
        //         npm install
        //         """
        //         }
        //     }
        // }
        stage('Pull image') {
            steps {
                sh "docker build . -t ${DOCKERTAG}"
            }
        }
        stage ("Attempting security stages") {
            steps {
                shared()
            }
        }
    }
}