pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest' // Pre-installed Ansible
        }
    }

    environment {
        INVENTORY = 'ansible/inventory.ini'
        PLAYBOOK = 'ansible/deploy.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/your-static-site.git'
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sh 'ansible-playbook -i $INVENTORY $PLAYBOOK'
            }
        }
    }

    post {
        success {
            echo '✅ Deployed to EC2 via Ansible!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
