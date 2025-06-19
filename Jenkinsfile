pipeline {
    agent {
        docker {
            image 'willhallonline/ansible:latest'
            args '-v /root/.ssh:/root/.ssh:ro'
        }
    }

    environment {
        REPO_DIR = 'ci-cd-repo'
        SSH_KEY = credentials('ssh-key') // Set this in Jenkins > Credentials
        HOME = '/tmp' // Fix Ansible tmp permission issue
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Verify Structure') {
            steps {
                dir("${REPO_DIR}/ansible") {
                    sh '''
                        echo "PWD in Jenkins inside container: $(pwd)"
                        echo "Listing all files:"
                        ls -la
                    '''
                }
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sshagent(['ssh-key']) {
                    dir("${REPO_DIR}/ansible") {
                        sh '''
                            echo "Running Ansible from: $(pwd)"
                            ansible-playbook -i inventory.ini nginx_deploy.yml
                        '''
                    }
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
