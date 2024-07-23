# Git Action Setup for SCP & SSH

## Step 1

| create a file at main dir of repo `.github/workflows/<file_name>.yml

## Step 2

Use the below code for .yml and set the secret & variable keys at the settings of Github repo

```yml
name: Build Deployment to Prod
on:
  push: 
    branches: 
      - <your-branch-name>
    
# test line
jobs:
 deploy-build:
   runs-on: ubuntu-latest
   steps:
     - uses: actions/checkout@v3
     - run: sudo apt-get install zip
     - run: zip -r build.zip build
     - run: pwd
     - run: ls -la
       name: Deploying the latest build
     - uses: a-was/scp-action@v1
       with:
        # Remote host
          host: ${{ vars.SERVER_HOST }}
          # Remote host SSH port. Default 22
          port: 22
          # Remote host user
          user: ${{ vars.SERVER_USERNAME }}
          # SSH private key
          key: ${{ secrets.SERVER_PRIVATE_KEY }}
          # Repo directory path. Default whole repo
          source: 'build.zip'
          # Remote directory path. Default home directory (~/)
          destination: '${{vars.ROOT_PATH}}'
      
     - uses: appleboy/ssh-action@master
       with:
        # Remote host
            host: ${{ vars.SERVER_HOST }}
            # Remote host SSH port. Default 22
            port: 22
            # Remote host username
            username: ${{ vars.SERVER_USERNAME }}
            # SSH private key
            key: ${{ secrets.SERVER_PRIVATE_KEY }}
            script: |
                cd ${{vars.ROOT_PATH}}
                ls -la
                rm -fr build
                unzip -o build.zip
                rm -fr 	${{vars.PROJECT_PATH}}/build/*
                cp -fr build/* ${{vars.PROJECT_PATH}}/build/
                cd ${{vars.PROJECT_PATH}}/build/ && ls -la
                sudo systemctl restart nginx

```

### Note

| Replace the key's with based on your requirement values

Example: 
              
- `<file_name>` : build_deployment.yml
- `<your-branch-name>` : stagging


### Secret & variables Keys as mention above
| Key  | Action key type | Example | Description |
| ------------- | ------------- | --------- |------------- |
| SERVER_HOST  | variable  | 127.0.0.1 | Host IP of the server |
| SERVER_USERNAME | variable  | ubuntu | server access username |
| SERVER_PRIVATE_KEY | secret  | -----BEGIN RSA PRIVATE KEY--*******-----END RSA PRIVATE KEY-----| SSH Private key to access the server |
| ROOT_PATH | variable  | ~/chanduarepalli | This is for the destination of file to store in server. `build.zip` is the file acording to our usecase|
| PROJECT_PATH | variable  | /home/chanduarepalli/demo_project  | This is the final build destination of our project |
