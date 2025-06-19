pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:debian'
            args '-u root'  // ensures SSH access and permissions
        }
    }

    environment {
        INVENTORY = 'ansible/inventory.ini'
        PLAYBOOK = 'ansible/nginx_deploy.yml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Pandu921/CI-CD.git'
            }
        }

        stage('Verify Setup') {
            steps {
                sh '''
                    echo "🔍 Verifying Ansible..."
                    ansible --version || echo "❌ Ansible not found!"
                    echo "🔍 Verifying inventory file:"
                    cat $INVENTORY
                '''
            }
        }

        stage('Deploy to EC2 with Ansible') {
            steps {
                sshagent (credentials: ['ec2-ssh-key']) {
                    sh '''
                        echo "🚀 Running Ansible Playbook"
                        ansible-playbook -i $INVENTORY $PLAYBOOK
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully!'
        }
        failure {
            echo '❌ Deployment failed. Check logs.'
        }
    }
}
