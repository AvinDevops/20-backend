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
        stage('test') {
            steps {
                sh """
                    echo "this is testing"
                    ls -ltr
                """
            }
        }
        
    }
    post {
        always {
            echo 'I will always run.'
            deleteDir()
        }
        success {
            echo 'I will run only when pipeline is success'
        }
        failure {
            echo 'I will run only when pipeline is failure'
        }
    }
}