pipeline{ 
    agent any 
        stages{ 
            stage("Build"){ 
                steps{ 
                    ```sh
                    echo "Start Building" 
                    npm install
                    npm run build 
                    echo "Building Completed"
                    ``` 
                    } 
                }
            stage("Deploy"){
                steps{
                    echo "Staring Deployment on EC2"
                    sh "scp -r -o strictCheckingOfKey=No ./dist/* /home/ubuntu/node-app/"
                    sh "npm start"
                    echo "Deployment done"
                }
            } 
            } 
        }