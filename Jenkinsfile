pipeline{
    agent any

    environment{
        EC2_HOST = "3.111.38.86"        //public ip of ec2 server
        SSH_CREDENTIAL_ID = "ec2-key"    //.pem file content
        REMOTE_USER = "ubuntu"  //username of the ec2 machine
        REMOTE_PATH = "/home/ubuntu/app" 
        WEB_ROOT = "/var/www/html"
        SERVER = "nginx"
    }

    stages{
        stage("Build"){
            steps{
                echo "Installing Dependencies"
                sh "npm install"
                echo "Building the app"
                sh "npm run build"
                echo "Build complete"
            }
        }
        stage("Deploy"){
            steps{
                echo "Starting Deployment"
                sshagent(credential: [env.SSH_CREDENTIAL_ID]){
                    sh """
                        echo "Creating Remote Directory"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} 'mkdir -p ${REMOTE_PATH}'

                        echo "Copying Distribution to Remote Path"
                        scp -o StrictHostKeyChecking=no -r dist/* ${REMOTE_USER}@${EC2_HOST}:${REMOTE_PATH}/

                        echo "Restarting Nginx"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST}'
                            sudo rm -rf ${WEB_ROOT}/*
                            sudo cp -r ${REMOTE_PATH}/* ${WEB_ROOT}/
                            sudo systemctl restart ${SERVER}
                        '
                    """
                }
                
            }
        }
    }
}