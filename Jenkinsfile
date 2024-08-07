pipeline {
    agent any
    
    environment {
        // Add your credentials and org-specific details here
        CLIENT_ID = '3MVG9fe4g9fhX0E5LtKVD2LmourXdGJM23GOJgTC.3naYqPKtmgHdIXvfVw3B3KLgUnHFL9B9tfXwC7w2c6PP'
        USERNAME = 'babanpawar7387@gmail.com'
        INSTANCE_URL = 'https://login.salesforce.com'
        JWT_KEY_FILE = credentials('jwt_key_file') // Assumes a Jenkins credential ID named 'jwt_key_file'
        TARGET_ORG_ID = '00D5g00000A4uMYEAZ'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Authorize Salesforce') {
            steps {
                script {
                    bat """
                    "C:\\Program Files\\sf\\bin\\sf.cmd" force:auth:jwt:grant --client-id ${CLIENT_ID} --username ${USERNAME} --jwt-key-file ${JWT_KEY_FILE} --set-default-dev-hub --instance-url ${INSTANCE_URL}
                    """
                }
            }
        }
        
        stage('Deploy Code') {
            steps {
                script {
                    bat """
                    "C:\\Program Files\\sf\\bin\\sf.cmd" project deploy start -d manifest/ --target-org ${TARGET_ORG_ID}
                    """
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
