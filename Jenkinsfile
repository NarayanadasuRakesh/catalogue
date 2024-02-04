pipeline {
    agent {
        node { label 'node1' }
    }
    environment {
        packageVersion = ''
        nexusURL = credentials('nexusURL') //Congifure inside manage credentials section
        environment = 'dev'
    }
    options {
        ansiColor('xterm') //ansiColor plugin
    }
    stages {
        stage('Get the version') {
            steps {
                sh '''
                    cd catalogue
                    ls -la
                '''
                script { //Pipeline utility steps plugin
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "Application version: $packageVersion"
                }
            }
        }
        stage('Install dependencies') {
            steps {     //Install nodejs in agent/node
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
        stage('upload artifact') { //Nexus Artifact Uploader plugin
            nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: "${env.nexusURL}",
                groupId: 'com.robotshop',
                version: "${packageVersion}", //Updates nexus repository with new sematic version
                repository: 'catalogue',
                credentialsId: 'nexus-auth', //Congifure inside manage credentials section
                artifacts: [
                    [artifactId: 'catalogue',
                    classifier: '',
                    file: 'catalogue.zip',
                    type: 'zip']
                ]
            )
        }
        stage('Trigger-deploy-job') {
            steps {
                script {
                    def parms = [
                        string(name: 'version',value: "${packageVersion}")
                        string(name: 'environment',value: "${environment}")
                    ]
                    build job: "catalogue-deploy", wait: true, paramaters:params //triggers catalogue-deploy job
                }
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