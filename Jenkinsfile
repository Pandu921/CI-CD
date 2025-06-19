pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            args '-v /root/.ssh:/root/.ssh:ro'
        }
    }

    environment {
        SSH_KEY = credentials('ssh-key') // Jenkins > Credentials
        HOME = '/tmp' // Fix Ansible tmp permission
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Print Workspace & Verify') {
            steps {
                sh '''
                    echo "Jenkins WORKSPACE: $WORKSPACE"
                    echo "Directory Tree:"
                    find $WORKSPACE
                '''
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sshagent(['ssh-key']) {
                    sh '''
                        cd $WORKSPACE/ci-cd-repo/Ansiblefiles
                        echo "Current Dir: $(pwd)"
                        ls -l
                        ansible-playbook -i inventory.ini nginx_deploy.yml
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployed successfully!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
