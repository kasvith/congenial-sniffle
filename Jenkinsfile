pipeline {
    agent any
    environment {
        CI = 'true'
        API = 'fooAPI'
    }
    stages {
        stage('Setup APIM Environments'){
            steps{
                sh './config.sh'
            }
        }
        stage('Deploy to Test') {
            environment{
                ENV = 'test'
                RETRY = '60'
            }
            steps {
                echo 'Deploying to Test'
                withCredentials([usernamePassword(credentialsId: 'apim', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'apimcli import-api -f $API -e $ENV -u $USERNAME -p $PASSWORD -k --preserve-provider=false --update --inject --verbose'
                }
            }
        }
        stage('Deploy to Production') {
            when{
                buildingTag()
            }
            environment{
                ENV = 'production'
            }
            steps {
                echo 'Deploying to Production'
                withCredentials([usernamePassword(credentialsId: 'apim', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'apimcli import-api -f $API -e $ENV -u $USERNAME -p $PASSWORD -k --preserve-provider=false --update --inject --verbose'
                }
            }
        }
    }
}