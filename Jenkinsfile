pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time:30, unit:'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    stages {
         stage('read the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    def appVersion = packageJson.version
                    echo "application version: $appVersion"
                }            
            }
        }
        stage('Install Dependencies') {
            steps {
                sh """
                    npm install
                    ls -ltr
                    echo "app version: $appVersion"
                """
            }
        }
       
        
    }
    post {
        always {
            echo 'I will always run.'
            //deleteDir()
        }
        success {
            echo 'I will run only when pipeline is success'
        }
        failure {
            echo 'I will run only when pipeline is failure'
        }
    }
}