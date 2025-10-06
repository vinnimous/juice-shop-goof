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

        stage('Snyk Scan') {
            steps {
                script {
                    echo "Downloading Snyk agent"
                    sh '''
                        snyk_cli_dl_linux="https://static.snyk.io/cli/latest/snyk-linux"
                        echo "Download URL: ${snyk_cli_dl_linux}"
                        curl -Lso ./snyk "${snyk_cli_dl_linux}"
                        chmod +x snyk
                        '''
                    //Authenticate Snyk
                    echo "Authorizing Snyk"
                    withCredentials([string(credentialsId: 'snyk_cli', variable: 'secretText')]) {
                        sh "./snyk auth ${secretText}"
                    }
                    // Run Snyk Open Source test
                    // This will scan for vulnerabilities in your dependencies.
                    echo "Snyk Open Source Test"
                    sh 'snyk test'

                    // Run Snyk Code test (for static application security testing)
                    // Requires Snyk Code to be enabled for your organization
                    echo "Snyk Code Test"
                    sh 'snyk code test'
                }
            }
        }
    }
}
