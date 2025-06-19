pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
        }
    }

    environment {
        INVENTORY = 'ansible/inventory.ini'
        PLAYBOOK = 'ansible/deploy.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Pandu921/CI-CD.git'
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sshagent (credentials: ['ec2-ssh-key']) {
                    sh '''
                        ansible-playbook -i $INVENTORY $PLAYBOOK
                    '''
                }
            }
        }
    }
}
