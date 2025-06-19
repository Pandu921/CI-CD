pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            args '-u root'
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
                    apt update && apt install -y git
                    git --version
                '''
            }
        }

        stage('Verify Files') {
            steps {
                sh '''
                    echo "📁 Checking ansible directory:"
                    ls -la ansible/
                    cat $INVENTORY
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: ['ec2-ssh-key']) {
                    sh '''
                        ansible-playbook -i $INVENTORY $PLAYBOOK
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment done!'
        }
        failure {
            echo '❌ Failed. Check logs.'
        }
    }
}
