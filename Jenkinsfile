pipeline {
    agent any // Use any available Jenkins agent
    
    environment {
        JWT_KEY_FILE = credentials('16dac807-3c9e-4e29-84af-526ab4a3e0ac') // Updated Jenkins credential ID
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
                    "C:\\Program Files\\sf\\bin\\sf.cmd" force:auth:jwt:grant --client-id 3MVG9fe4g9fhX0E5LtKVD2LmourXdGJM23GOJgTC.3naYqPKtmgHdIXvfVw3B3KLgUnHFL9B9tfXwC7w2c6PP --username babanpawar7387@gmail.com --jwt-key-file ${env.JWT_KEY_FILE} --set-default-dev-hub --instance-url https://login.salesforce.com
                    """
                }
            }
        }
        
        stage('Deploy Code') {
            steps {
                script {
                    bat """
                    "C:\\Program Files\\sf\\bin\\sf.cmd" project deploy start -d manifest/ --target-org 00D5g00000A4uMYEAZ
                    """
                }
            }
        }
    }
    
    post {
        always {
            node {
                cleanWs() // Clean workspace within a node block
            }
        }
    }
}
