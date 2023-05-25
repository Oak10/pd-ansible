pipeline {
    triggers {
        pollSCM('H/4 * * * *')
    }
    environment {
        DEPLOY_DIR='pd-ansible'
        DEPLOY_USER='ubuntu'
        DEPLOY_HOST='localhost'
        WEBHOOK_URL = credentials('TEAMS_WEBHOOK')
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '2'))
        skipDefaultCheckout()
    }
    agent any
    stages {
        stage('Update Ansible playbooks') {
            when {
                branch "master"
            }
            steps {
                sshagent(['ubuntulocalhost']) {
                    sh "ssh -l $DEPLOY_USER $DEPLOY_HOST 'cd $DEPLOY_DIR && git fetch && git pull'"
                }
    
            }
        }
    }
    post { 
        always {
            office365ConnectorSend webhookUrl: "${env.WEBHOOK_URL}",
            message: "[Job: `${env.JOB_NAME}` - #${env.BUILD_NUMBER}]: ${env.BUILD_URL}",
            status: "${currentBuild.result}"
            }
        }
}