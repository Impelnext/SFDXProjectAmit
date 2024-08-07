pipeline {
    agent any
    
    environment {
        JWT_KEY_FILE = credentials('jwt_key_file') // Assumes a Jenkins credential ID named 'jwt_key_file'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Authorize Salesforce') {
            steps {
                bat """
                "C:\\Program Files\\sf\\bin\\sf.cmd" force:auth:jwt:grant --client-id 3MVG9fe4g9fhX0E5LtKVD2LmourXdGJM23GOJgTC.3naYqPKtmgHdIXvfVw3B3KLgUnHFL9B9tfXwC7w2c6PP --username babanpawar7387@gmail.com --jwt-key-file ${JWT_KEY_FILE} --set-default-dev-hub --instance-url https://login.salesforce.com
                """
            }
        }
        
        stage('Deploy Code') {
            steps {
                bat """
                "C:\\Program Files\\sf\\bin\\sf.cmd" project deploy start -d manifest/ --target-org 00D5g00000A4uMYEAZ
                """
            }
        }
    }
    
    post {
        always {
            node {
                cleanWs()
            }
        }
    }
}
