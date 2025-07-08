pipeline {
    agent any

    environment {
        EC2_HOST = '13.233.11.178'              // Replace with your target EC2 public IP
        SSH_CREDENTIAL_ID = 'ec2-node-key'       // The ID you gave in Jenkins Credentials
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu/app'        // Temp folder to hold build
        WEB_ROOT = '/var/www/html'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'

                echo 'Building the application...'
                sh 'npm run build'
                echo 'Build complete.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to EC2 at ' + env.EC2_HOST'
                sshagent (credentials: [env.SSH_CREDENTIAL_ID]){
                sh """
                        echo "Creating remote directory..."
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} 'mkdir -p ${REMOTE_PATH}'

                        echo "Copying build to remote EC2..."
                        scp -o StrictHostKeyChecking=no -r dist/* ${REMOTE_USER}@${EC2_HOST}:${REMOTE_PATH}/

                        echo "Moving files to web root and restarting nginx..."
                        ssh ${REMOTE_USER}@${EC2_HOST} '
                            sudo rm -rf ${WEB_ROOT}/*
                            sudo cp -r ${REMOTE_PATH}?* ${WEB_ROOT}/
                            sudo systemctl restart nginx
                        '
                    """
                }

                echo 'Deployment completed.'
            }
        }
    }
}