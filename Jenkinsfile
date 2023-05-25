pipeline {
    triggers {
        pollSCM('H/4 * * * *')
    }
    environment {
        DEPLOY_DIR='pd-ansible'
        DEPLOY_USER='ubuntu'
        DEPLOY_HOST='localhost'
        WEBHOOK_URL = "https://isecpt.webhook.office.com/webhookb2/f350ae6c-4b41-4b65-a873-bbe325c74e39@055a3822-099b-4683-ad90-b17a3b3b1507/JenkinsCI/635bb3f1f20143618a91132d005bbc25/3a8381ac-1f46-4be0-a9c3-ac901668c9e2"
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '2'))
        skipDefaultCheckout()
        office365ConnectorWebhooks([
            [name: "Office 365", 
            url: '${WEBHOOK_URL}',
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