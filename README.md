# [CI/CD Pipeline Using GitHub Action Deploy Nodejs Project on AWS EC2 (ubuntu) step by step](https://github.com/jmalamin40/nodejs_deploy_in_aws_cicd_pipeline)

*Now we can deploy nodejs project in aws EC2*
## Documention
### Step 01
* create a node js project in the local machine
* confirm your project that is perfectly working on the local machine 
### Step 02
* [login AWS console](https://console.aws.amazon.com/console/home?nc2=h_ct&src=header-signin)  
* create a new IAM user role/if exist then ok. when you generate a new key for the pair must be downloaded and stored in your local machine 

* select the region when you want to create a server 
* go to EC2 instance  
* create a new security group/if exist then ok. ensure your inbound specific port is allowed for public traffic where you want to run your nodejs project 
* create key pair for connecting to remote ssh /if exist then ok 
* as per your requirement create your server 

### Step 03
* now you need to connect your server via ssh key 
(you can use PuTTYgen or anything)  

### Step 04:
* now go to GitHub and create a new repository for your nodejs project/if exist then ok 

### Step 05
* Now push your nodejs project in this repository 

### Step 06
 * create workflow in your project  
 * create a node.js.yml file in this directory (.github/workflows/node.js.yml) 

 * import this code 

        ```name: Node.js CI

            on:
            push:
                branches: [ "main" ] 

            jobs:
            build:

                runs-on: self-hosted

                strategy:
                matrix:
                    node-version: [12.x]
                    

                steps:
                - uses: actions/checkout@v3
                - name: Use Node.js ${{ matrix.node-version }}
                uses: actions/setup-node@v3
                with:
                    node-version: ${{ matrix.node-version }}
                    cache: 'npm'
                - run: npm ci
                - run: npm run build --if-present
                - run: pm2 restart index.js
        ```

### Step 07:
    *  go to your github repository and go to Settings
 after that open Actions tab from left side navigation  and click the Runners menu. Then click new self-hosted runner.
    * select your OS Linux then scroll down 

    * open your server via ssh connection and follow them 
    * Create a folder for this project 
        #  $ mkdir actions-runner && cd actions-runner 
    
    *  Download the latest runner package 
        *  $ curl -o actions-runner-linux-x64-2.304.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.304.0/actions-runner-linux-x64-2.304.0.tar.gz 
    
    
    *  Extract the installer 
        *  $ tar xzf ./actions-runner-linux-x64-2.304.0.tar.gz 
    
    *  Create the runner and start the configuration experience 
        *  $ ./config.sh --url https://github.com/jmalamin40/nodejs_deploy_in_aws_cicd_pipeline --token copy from github 
    
    * install this bash 
        * $  sudo ./svc.sh install 

    * start 
        * $  sudo ./svc.sh start 
    
    * install nodejs and database as per your project requirement 


    * Use this YAML in your workflow file for each job 
        * runs-on: self-hosted 

    * now push your targeted branch  

    * ensure your project port is allow for public traffic in aws security  group 

### Step 08
    * check your github action  log 

