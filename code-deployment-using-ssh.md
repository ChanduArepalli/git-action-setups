# Git Action Setup for SSH

## Step 1

| create a file at main dir of repo `.github/workflows/<file_name>.yml

## Step 2

Use the below code for .yml and setup the secret & variable keys at the settings of Github repo

```yml
name: Webhook Deploy to Tenant Dev
on:
  push: 
    branches: 
      - <your-branch-name>

jobs:
 deploy-build:
   runs-on: ubuntu-latest
   steps:
     - uses: actions/checkout@v3
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
          # Scripts to execute the CMD's in the server
          script: |
            cd ${{vars.PROJECT_PATH}}/${{vars.PROJECT_NAME}}
            pwd
            sudo git checkout <your-branch-name>
            sudo git pull origin <your-branch-name>
            echo "Restart - Gunicorn service - ${{vars.GUNICORN_SERVICE}}"
            sudo systemctl restart ${{vars.GUNICORN_SERVICE}}
            echo "Restart - Nginx"
            sudo systemctl restart nginx

```

### Note

| Replace the key's with based on your requirement values

Example: 
- `<your-branch-name>` : production
- `<file_name>` : server_deployment.yml
