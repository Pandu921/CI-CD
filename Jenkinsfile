pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            args '-v /root/.ssh:/root/.ssh:ro'
        }
    }

    environment {
        SSH_KEY = credentials('ssh-key') // Jenkins > Credentials
        HOME = '/tmp'                    // To avoid Ansible permission issue
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
                sshagent(['ssh-key']) {
                    sh '''
                        cd $WORKSPACE/ci-cd-repo/Ansiblefiles
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
