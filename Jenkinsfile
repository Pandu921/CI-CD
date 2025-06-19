pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            // Explicitly override user to root so SSH can run without UID issues
            args '--user=root -v /root/.ssh:/root/.ssh:ro'
        }
    }

    environment {
        SSH_KEY = credentials('ssh-key')   // your SSH credential ID in Jenkins
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
