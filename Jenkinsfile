pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time:30, unit:'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment {
        def appVersion = '' //variable declaration
        nexusUrl = 'nexus.avinexpense.online:8081'
    }
    stages {
         stage('read the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
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
        stage('Build') {
            steps {
                sh """
                    zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                    ls -ltr
                """
            }
        }

        stage('Sonar Scan') {
            environment {
                scannerHome = tool 'Sonar 7.0' //referring agnet software
            }
            steps {
                script {
                    withSonarQubeEnv('Sonar 7.0') { //referring sonar
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Nexus artifact upload') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusUrl}",
                    groupId: 'com.expense',
                    version: "${appVersion}",
                    repository: 'backend',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: "backend",
                        classifier: '',
                        file: 'backend-' + "${appVersion}" + '.zip',
                        type: 'zip']
                    ]
                )
            }
        }
        stage ('Starting downstream job ') {
            steps {
                script {
                    def params = [
                        string(name: 'appVersion', value: "${appVersion}")
                    ]
                     build job: 'backend-deploy', parameters: params, wait: false
                }
               
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