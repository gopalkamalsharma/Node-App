pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'
        EC2_HOST = '13.232.208.28'
        SSH_CREDENTIAL_ID = 'ec2-ssh-key'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Start Building'
                sh 'npm install'
                sh 'npm run build'
                echo 'Building Completed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting Deployment to EC2...'
                // Use withCredentials to inject SSH private key
                sshagent(['ec2-ssh-key']) {
                    sh '''
                        echo "Copying build artifacts to EC2"
                        scp -o StrictHostKeyChecking=no -r dist/ ubuntu@13.232.208.28:/var/www/html
                        echo "Restarting web server on EC2"
                        ssh -o StrictHostKeyChecking=no ubuntu@13.232.208.28 'sudo systemctl restart nginx'
                    '''
                }
                echo 'Deployment Completed'
            }
        }
    }
}