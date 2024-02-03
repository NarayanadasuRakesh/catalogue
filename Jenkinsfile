pipeline {
    agent {
        node { label 'Node-1' }
    }
    environment {
        packageVersion = ''
    }
    options {
        ansiColor('xterm')
    }
    stages {
        stage('Get the version') {
            steps {
                sh '''
                    cd catalogue
                    ls -la
                '''
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "Application version: $packageVersion"
                }
            }
        }
        stage('Install dependencies') {
            steps {
                sh '''
                    cd catalogue
                    npm install
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    cd catalogue
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                ''' 
            }
        }
    }
    post {
        always {
            echo "Deleting directory"
            deleteDir()
        }
    }
}