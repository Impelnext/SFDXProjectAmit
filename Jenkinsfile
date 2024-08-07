#!groovy
import groovy.json.JsonSlurperClassic

node {

    def BUILD_NUMBER = env.BUILD_NUMBER
    def SFDC_USERNAME = env.SFDC_USERNAME
    def HUB_ORG = env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY = env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS'
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    
    // Specify the correct path to the Salesforce CLI tool
    def toolbelt = "C:\\Program Files\\sf\\bin\\sf.cmd"

    stage('checkout source') {
        checkout scm
    }

withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
    stage('Deploy Code') {
        if (isUnix()) {
            rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --client-id ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwt-key-file ${jwt_key_file} --set-default-dev-hub --instance-url ${SFDC_HOST}"
        } else {
            rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --client-id ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwt-key-file \"${jwt_key_file}\" --set-default-dev-hub --instance-url ${SFDC_HOST}"
        }
        if (rc != 0) { error 'hub org authorization failed' }

        println rc

        // Updated command for deployment
        if (isUnix()) {
            rmsg = sh returnStdout: true, script: "${toolbelt} project deploy start -d manifest/ -u ${HUB_ORG}"
        } else {
            rmsg = bat returnStdout: true, script: "\"${toolbelt}\" project deploy start -d manifest/ -u ${HUB_ORG}"
        }

        println rmsg
    }
    }
}
