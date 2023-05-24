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
}