@Library("security_stages") _

pipeline {
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
                sh "docker pull boosef-juiceshop:latest"
            }
        }
        stage('Snyk Setup') {
        
        
            steps {
                echo "Downloading Snyk agent"
                sh '''
                    snyk_cli_dl_linux="https://static.snyk.io/cli/latest/snyk-linux"
                    echo "Download URL: ${snyk_cli_dl_linux}"
                    curl -Lso ./snyk "${snyk_cli_dl_linux}"
                    chmod +x snyk
                    '''
                //Authenticate Snyk
                echo "Authorizing Snyk"
                withCredentials([string(credentialsId: 'snyk_cli_2', variable: 'secretText')]) {
                    sh "./snyk auth ${secretText}"
                }
            }
        }
        stage('Snyk Open Source'){
            // when {
            //     branch "master"
            // }
            steps {
                catchError(stageResult: 'UNSTABLE', buildResult: 'UNSTABLE', message: 'Snyk Open Source has issues') {
                    echo "Snyk Open Source Starting"
                    sh '''
                        ./snyk test --all-projects
                        ./snyk monitor
                    '''
                    echo "Snyk Open Source Complete"
                } 
            }
        }
        stage('Snyk Code'){
            // when {
            //     branch "master"
            // }
            steps {
                catchError(stageResult: 'UNSTABLE', buildResult: 'UNSTABLE', message: 'Snyk Code has issues') {
                    echo "Snyk Code Starting"
                    sh '''
                        ./snyk code test
                    '''
                    echo "Snyk Code Complete"
                } 
            }
        }
        stage('Snyk Container'){
            // when {
            //     branch "master"
            // }
            steps {
                catchError(stageResult: 'UNSTABLE', buildResult: 'UNSTABLE', message: 'Snyk Container has issues') {
                    echo "Snyk Container Starting"
                    sh '''
                        ./snyk container test boosef-juiceshop:latest --file=Dockerfile
                        ./snyk container monitor boosef-juiceshop:latest --file=Dockerfile
                    '''
                    echo "Snyk Container Complete"
                } 
            }
        }
        stage('Snyk IAC'){
            // when {
            //     branch "master"
            // }
            steps {
                catchError(stageResult: 'UNSTABLE', buildResult: 'UNSTABLE', message: 'Snyk IAC has issues') {
                    echo "Snyk IAC Starting"
                    sh '''
                        ./snyk iac test --report
                    '''
                    echo "Snyk IAC Complete"
                } 
            }
        }
        // stage ("Attempting security stages") {
        //     steps {
        //         shared()
        //     }
        // }
    }
}