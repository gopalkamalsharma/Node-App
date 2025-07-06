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
            } 
        }