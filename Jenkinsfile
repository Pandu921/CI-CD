pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            // Removed the invalid -u 105:109 and kept only the SSH volume mount
            args '-v /root/.ssh:/root/.ssh:ro'
        }
    }

    environment {
        SSH_KEY = credentials('ssh-key')
        HOME = '/tmp'
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Print Structure') {
            steps {
                sh '''
                    echo "Jenkins WORKSPACE: $WORKSPACE"
                    echo "List structure under workspace:"
                    find $WORKSPACE
                '''
            }
        }

        stage('Deploy via Ansible') {
            steps {
                // Use Jenkins SSH agent to inject private key for SSH
                sshagent(['ssh-key']) {
                    sh '''
                        cd $WORKSPACE/Ansiblefiles
                        echo "📂 Running from $(pwd)"
                        ls -l
                        ansible-playbook -i inventory.ini nginx_deploy.yml
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
