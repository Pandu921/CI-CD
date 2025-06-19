pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            args '-u root' // run as root to install packages
        }
    }

    environment {
        INVENTORY = 'ansible/inventory.ini'
        PLAYBOOK = 'ansible/nginx_deploy.yml'
    }

    stages {
        stage('Install Git') {
            steps {
                sh '''
                    echo "🔧 Installing Git..."
                    apt update && apt install -y git
                    git --version
                '''
            }
        }

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Pandu921/CI-CD.git'
            }
        }

        stage('Verify Files') {
            steps {
                sh '''
                    echo "📁 Contents of ansible directory:"
                    ls -la ansible/
                    echo "📄 Inventory file:"
                    cat $INVENTORY
                '''
            }
        }

        stage('Deploy to EC2 with Ansible') {
            steps {
                sshagent (credentials: ['ec2-ssh-key']) {
                    sh '''
                        echo "🚀 Running Ansible Playbook..."
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
            echo '❌ Deployment failed. Check logs above.'
        }
    }
}
