pipeline {
    triggers {
        pollSCM('H/4 * * * *')
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '2'))
        skipDefaultCheckout()
    }
    environment {
        DEPLOY_DIR='pd-ansible'
        DEPLOY_USER='ubuntu'
        DEPLOY_HOST='localhost'
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
            withCredentials([string(credentialsId: 'TEAMS_WEBHOOK', variable: 'TEAMS_WEBHOOK')]) { 
                office365ConnectorSend webhookUrl: '${TEAMS_WEBHOOK}',
                message: "${currentBuild.result}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}",
                status: "${currentBuild.result}"
            }  
        }
    }

}