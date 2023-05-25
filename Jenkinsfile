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
            script {
                echo 'Always'
                if(currentBuild.result == "SUCCESS"){
                    env.WEBHOOK_COLOUR = "3cb371"
                } else {
                    env.WEBHOOK_COLOUR = "ff0000"
                }
            }
            office365ConnectorSend webhookUrl: '$WEBHOOK_URL',
            message: "`${env.JOB_NAME}` #${env.BUILD_NUMBER}: ${env.BUILD_URL}",
            color: "${WEBHOOK_COLOUR}"
            status: "${currentBuild.result}"
            }
        }
}