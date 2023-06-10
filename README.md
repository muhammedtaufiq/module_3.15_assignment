# module_3.15_assignment

# 3.13_assignment-homework
assignment submission.
added aws secrets
### Brief

### Brief

The objective of this assignment is to set up a continuous integration
and continuous deployment (CI/CD) pipeline for a Node.js application
that is deployed on a serverless platform with Secret Management in
placed!.

Instructions:

1.  Create a new Node.js **application** using the Express framework.

2.  Set up **automatic testing** for the application using a testing
    framework such as Jest or Mocha.

3.  Create **new branch** on your existing repository.

4.  Set up a CI/CD pipeline using a tool such as Github Action, Jenkins,
    Travis CI, or CircleCI.

5.  Configure the pipeline to automatically build and test the code
    whenever changes are pushed to the code repository.

6.  Deploy the application to a serverless platform such as AWS Lambda
    or Google Cloud Functions.

7.  **Store some information using Secret Management on AWS, then get
    the information as part of the pipeline.**

8.  Configure the pipeline to automatically deploy the code to the
    serverless platform whenever changes are pushed to the code
    repository and all tests pass.

9.  Add some logging on your application

10. Retrieve your logging information from AWS Cloudwatch

11. Add that logging information on new file and commit it to your github repo 

12. Write a documentation explaining the steps you have done, including
    the tools and services used.

Deliverables:

-   The Node.js application with automatic testing set up

-   Code repository with the CI/CD pipeline configured

-   Documentation explaining the steps taken to set up the pipeline.

# 

# 

# Step 1 -- create github repository

![1](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/35593188-6360-4214-8f15-57b7dcddf467)


# Step 2 -- create a local folder on your local machine

We will clone into here at the next step




# Step 3 -- Clone repo to local host-folder created at the step above

a)  Open terminal in the folder above

![2](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/b88dc78f-7460-4598-8290-413cd13cedfe)


b)  \$ git clone https://github.com/muhammedtaufiq/module_3.15_assignment.git


![3](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/a6549efb-420b-4827-b6d5-013f4b3ea8e3)


#ensure you cd into the git project root

# Step 4 -- creating index.js file to hold my application

![4](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/6b68d2d1-e883-4ded-92b4-6f3096bbd86b)

![4a](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/c8cfa38b-5d8f-462a-9eaa-916d4b1c2c59)

i used a random quote generator, you can use anything you like


# Step 5 create serverless .yml to contain instructions for serverless

Ref: [Serverless Framework
Services](https://www.serverless.com/framework/docs/providers/aws/guide/services/#:~:text=When%20you%20deploy%20a%20service%2C%20all%20functions%2C%20events,to%20dev%20stage%20and%20us-east-1%20region%20on%20AWS.)

Template :


![5](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/0d4ba1d2-6c50-43b9-8bad-ee14e522bf95)

service: Taufiq-module-3-15-assignment
frameworkVersion: '3'

provider:
  name: aws
  runtime: nodejs18.x
  region: ap-southeast-1

functions:
  api:
    handler: index.handler
    events:
      - httpApi:
          path: /
          method: get

plugins:
  - serverless-offline




# Step 6 -- install and deploy serverless application

**Install express**

**\$npm install -g express**

![9](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/68169b0d-2d83-4881-a4b3-c8da17df3981)



**Create json file**

**\$ npm init**
![Picture9](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/7dc6bb4b-b09a-44c0-ab13-e09eab185edd)

![7](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/c0c45867-6c64-4a63-8006-6315d9e18c86)


**\$ npm install**

![6](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/598807c5-a7ca-4c65-b7c7-c38d09dd5d65)
![8](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/1a5cc7d8-6425-4196-8460-0863873dfb1f)



Do ensure serverless offline plugin is installed:\


![10-serverless-offline](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/7029d360-e96f-40ee-ba2e-129528aba9b8)

TESTING SERVERLESS OFFLINE BEFORE DEPLOYMENT

**\$ npx serverless-offline**

![10-a-serverless-offlinetest](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/67a5ac37-78f7-4997-a858-cddb2501e13c)
![10a-serverless-offline](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/46b61d5f-b0aa-46bf-9ca4-7f65c6ca7cee)
![10b-serverless-offline](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/ff7768dc-dc23-44eb-a3ff-68e0208af14c)

**Deploying serverless**

**\$ serverless deploy**
![11-serverlessDEPLOY](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/90b9999f-433a-4e57-9aaa-579675b461a7)

![11a-serverlessDEPLOY](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/46cb225d-f52b-41f0-a368-328cd64e73fa)

**Acquire content from serverless to check its running**

You can now curl
<https://upkblf2yxc.execute-api.ap-southeast-1.amazonaws.com/> as shown
above under endpoint to see that its up on aws.

or alternatively open a browser and we see below random quote generated

{"quote":"Success is not final, failure is not fatal: It is the courage to continue that counts. - Winston Churchill"}

# Step 7 -- now that code is up and running on AWS- we create the CICD pipeline USING GitHub Actions to support changes.



Create main.yml in .github/workflows folder and use code below.
> **Create .github/workflows folder in root**
>
> **Create main.yml file in .github\workflows**
>
> ![12-github](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/1af4325c-3ebc-486c-a529-4a475a5fc0d5)

**---------------------------------------------------------**
name: CICD for Serverless Application
run-name: ${{ github.actor }} is doing CICD for Serverless Application

on:
  push:
    branches:  [ main, "*"]

jobs:
  pre-deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."

  install-dependencies:
    runs-on: ubuntu-latest
    needs: pre-deploy
    steps:
      - name: Check out repository code
        uses: actions/checkout@v3
      - name: Run Installation of Dependencies Commands
        run: npm install

  deploy:
    name: deploy
    runs-on: ubuntu-latest
    needs: install-dependencies
    strategy:
      matrix:
        node-version: [18.x]
    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - name: serverless deploy
      uses: serverless/github-action@v3.2
      with:
        args: deploy
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

----------------------------------------------------------------------------------
This code is a GitHub Actions workflow YAML file that defines a CI/CD pipeline for a serverless application. Here are the key aspects and what they do:

Workflow name: "CICD for Serverless Application" - It specifies the name of the workflow.

**on:** It defines the events that trigger the workflow. In this case, it triggers the workflow on pushes to the main branch and all other branches.

**jobs:** It defines the different jobs/stages of the CI/CD pipeline.

a. **pre-deploy:** This job runs on an Ubuntu environment and outputs some informational messages.

b. **install-dependencies:** This job checks out the repository code, then runs npm install to install project dependencies.

c. **deploy:** This job deploys the serverless application. It checks out the repository code, sets up the specified Node.js version, runs npm ci (clean install), and uses the serverless/github-action to deploy the application. It also sets the necessary AWS credentials using secrets.

Each job has a runs-on field that specifies the operating system (Ubuntu in this case). 
The needs field specifies any dependencies between jobs, ensuring that a job is executed only after its dependencies have successfully completed.

The matrix field within the deploy job allows for running the same job with multiple configurations, in this case, different versions of Node.js.

Overall, this GitHub Actions workflow sets up a CI/CD pipeline that automatically
triggers on pushes to the main branch and other branches, installs dependencies, and deploys a serverless application using the Serverless Framework.


Reference : [serverless/github-action: A Github Action for deploying
with the Serverless
Framework](https://github.com/serverless/github-action)

**#ensure the passwords are entered in github manager from AWS IAM**

**If you cant find it- create new keys if you are allowed on AWS IAM**

Or search in your respective local drives

<https://docs.aws.amazon.com/sdkref/latest/guide/file-location.html>

        AWS_ACCESS_KEY_ID: \${{ secrets.AWS_ACCESS_KEY_ID }}

        AWS_SECRET_ACCESS_KEY: \${{ secrets.AWS_SECRET_ACCESS_KEY }}

**%USERPROFILE%\\.aws\\config**
eg. c:\user\taufiq\.aws\config or credentials file

> **You will need this in the steps below**

# Step 8 Add secrets to Github Secrets -- ensure you are in git hub repo on github

Enter your aws keys here so that github can communicate with AWS
![Picture17](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/cdd8d56b-de99-4f19-83f4-53320b020ff4)

![13-githubsecrets](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/24551853-4c0f-4477-8aa9-34ef909c5b78)

![Picture18](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/ef442805-463c-4be3-8211-67c508138d9e)
![Picture19](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/061ac685-df40-4305-92ee-658f97b11c74)
![Picture20](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/1941299e-5f91-40a7-b4d2-cbce45c640d0)

# Step 9: push changes to Github to start the workflow

\$ git add .

\$ git commit -m "second commit"

\$ git push

\*\*\*\*ensure node js folder is installed and node_modules is included
in the .gitignore file
![Picture21](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/38a8afd6-0cdb-4800-a30b-abdb39a918ba)
![Picture22](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/a4c03923-08ca-4873-b8ed-c6479a21f996)



## Workflow started- check on Github Actions on Github as shown below

![13-githubrunningcicd](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/f45844d4-ebcb-41cf-9a41-937c87aeb274)

![Picture24](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/77a55d25-e35d-4369-9924-b1e2d0e3e177)



# Step 10 : add a secret in AWS secret manager

The goal of this exercise is to **Store some information using Secret
Management on AWS, then get the information as part of the pipeline.**
![Picture25](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/c11e1ce6-bd8f-4627-bd78-fbde056e250e)
![Picture26](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/a3d6d318-8c81-49e1-a0b5-eeeb10a1e3eb)


# Step 11- retrieve the secret from the AWS Secrets as part of the CICD pipeline

Add a new job **retrieve-secret** to

.github/workflows/main.yml to retrieve the stored
secret ***TAUFIQ_SECRET*** 

from AWS Secrets Manager and inject into environment variables
![Picture27](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/618cd1b6-1189-443a-9b42-98f019d95093)
![Picture28](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/7b0f5dc7-7734-4ed3-8dca-0be51f88b67d)
![14-retrieve](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/4dde8bb5-af1b-4fa4-ba47-e424152f8855)

Last 2 lines will print the secrets

**secrets**:

-   List of secret to be retrieved.

-   Examples:

    -   To retrieve a single secret, use secrets: my_secret_1

    -   To retrieve multiple secrets, use:

    -   secrets: \|

    -   my_secret_1

    -   my_secret_2

    -   To retrieve \"all secrets having names that contain dev\" or
        \"begin with app1/dev/\", use:

    -   secrets: \|

    -   \*dev\*

> app1/dev/\*

**parse-json:**

If parse-json: true and secret value is a valid stringified JSON object, it will be parsed and flattened. Each of the key value pairs in the flattened JSON object will become individual secrets. The original secret name will be used as a prefix.


# STEP 12-Push the changes to GitHub to start the workflow

![15-retrieve](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/354a4c76-ce7a-4a16-8973-0222ffc907ba)



# STEP 13-Adding logging to the serverless application

we add console.log to our index.js file to output this message "function was deployed successfully!!".
this message will be captured in *Cloudwatch* logs
![16-logging](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/e30f5bcc-b4ab-4204-8c78-50bbea78399f)


# STEP 14-push to git to start the CICD workflow again
git add .
git commit -m "yourmessage"
git push


# STEP 15- curl the below, or open in browser to see its successfully logging

*FIRST*
https://upkblf2yxc.execute-api.ap-southeast-1.amazonaws.com/
![18-logging](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/fd27d257-4ff6-405e-b4a9-3743756f2719)

*SECOND*

see the out put in cloudwatch.
Cloudwatch/log groups/aws/Lambda/Taufiq-module-3-15-assignment-dev.api

see screen grab below

![17-logging](https://github.com/muhammedtaufiq/module_3.15_assignment/assets/24953052/8e43563f-405b-4bd3-b9a8-22d491dd02f9)





