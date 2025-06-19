pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            args '-u root -v /root/.ssh:/root/.ssh:ro'
        }
    }

    environment {
        SSH_KEY = credentials('ssh-key')
    }

    stages {
        stage('Print Structure') {
            steps {
                sh '''
                    echo "Jenkins WORKSPACE: ${WORKSPACE}"
                    echo "List structure under workspace:"
                    find ${WORKSPACE}
                '''
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sshagent (credentials: ['ssh-key']) {
                    sh '''
                        cd ${WORKSPACE}
                        echo "📂 Running from $(pwd)"
                        
                        # Disable host key checking for Ansible
                        export ANSIBLE_HOST_KEY_CHECKING=False
                        
                        ansible-playbook -i Ansiblefiles/inventory.ini Ansiblefiles/nginx_deploy.yml
                    '''
                }
            }
        }
    }

    post {
        failure {
            echo '❌ Deployment failed!'
        }
        success {
            echo '✅ Deployment succeeded!'
        }
    }
}
