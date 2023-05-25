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
        office365ConnectorWebhooks([
            [name: "Office 365", 
            url: '$WEBHOOK_URL",
            notifyFailure: true, 
            notifySuccess: true]
        ])
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
}