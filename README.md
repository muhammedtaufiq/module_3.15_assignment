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



# Step 2 -- create a local folder on your local machine

We will clone into here at the next step




# Step 3 -- Clone repo to local host-folder created at the step above

a)  Open terminal in the folder above



b)  \$ git clone https://github.com/muhammedtaufiq/module_3.15_assignment.git




#ensure you cd into the git project root

# Step 4 -- creating index.js file to hold my application



module.exports.handler = async (event) =\> {

return {

statusCode: 200,

body: JSON.stringify(

{

message: \"Go Serverless v3.0! Your function executed successfully!\",

input: event,

},

null,

2

),

};

};

# Step 5 create serverless .yml to contain instructions for serverless

Ref: [Serverless Framework
Services](https://www.serverless.com/framework/docs/providers/aws/guide/services/#:~:text=When%20you%20deploy%20a%20service%2C%20all%20functions%2C%20events,to%20dev%20stage%20and%20us-east-1%20region%20on%20AWS.)

Template :




# Step 6 -- install and deploy serverless application

**Install express**

*\$npm install -g express*
![Picture8](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/24938558-7498-4e69-9bad-041d8a0ba815)



**Create json file**

*\$ npm init*
![Picture9](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/7dc6bb4b-b09a-44c0-ab13-e09eab185edd)


![Picture10](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/17abd9b9-77f2-40b2-9af8-5c82fc07beb8)


*\$ npm install*


![Picture11](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/253af680-5950-428a-b2e3-f5037a832487)

Do ensure serverless offline plugin is installed:\

![Picture12](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/8a539f76-3df6-44a8-81b9-83f67efc2086)

TESTING SERVERLESS OFFLINE BEFORE DEPLOYMENT

*\$ npx serverless-offline*


IMAGES IMAGES IMAGES


**Deploying serverless**

*\$ serverless deploy*
![Picture13](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/bde589bd-561e-489b-b523-9b8248a619fc)


**Acquire content from serverless to check its running**

You can now curl
<https://upkblf2yxc.execute-api.ap-southeast-1.amazonaws.com/> as shown
above under endpoint to see that its up on aws.

or alternatively open a browser and we see below random quote generated

{"quote":"Success is not final, failure is not fatal: It is the courage to continue that counts. - Winston Churchill"}

# Step 7 -- now that code is up and running on AWS- we create the CICD pipeline USING GitHub Actions to support changes.

a.  IMAGES IMAGES


Create main.yml in .github/workflows folder and use code below.


> *Create .github/workflows folder in root*
>
> *Create main.yml file in .github\workflows*
>
> IMAGES


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

*on:* It defines the events that trigger the workflow. In this case, it triggers the workflow on pushes to the main branch and all other branches.

*jobs:* It defines the different jobs/stages of the CI/CD pipeline.

a. *pre-deploy:* This job runs on an Ubuntu environment and outputs some informational messages.

b. *install-dependencies:* This job checks out the repository code, then runs npm install to install project dependencies.

c. *deploy:* This job deploys the serverless application. It checks out the repository code, sets up the specified Node.js version, runs npm ci (clean install), and uses the serverless/github-action to deploy the application. It also sets the necessary AWS credentials using secrets.

Each job has a runs-on field that specifies the operating system (Ubuntu in this case). 
The needs field specifies any dependencies between jobs, ensuring that a job is executed only after its dependencies have successfully completed.

The matrix field within the deploy job allows for running the same job with multiple configurations, in this case, different versions of Node.js.

Overall, this GitHub Actions workflow sets up a CI/CD pipeline that automatically
triggers on pushes to the main branch and other branches, installs dependencies, and deploys a serverless application using the Serverless Framework.


Reference : [serverless/github-action: A Github Action for deploying
with the Serverless
Framework](https://github.com/serverless/github-action)

#ensure the passwords are entered in github manager from AWS IAM

If you cant find it- create new keys if you are allowed on AWS IAM

Or search in your respective local drives

<https://docs.aws.amazon.com/sdkref/latest/guide/file-location.html>

        AWS_ACCESS_KEY_ID: \${{ secrets.AWS_ACCESS_KEY_ID }}

        AWS_SECRET_ACCESS_KEY: \${{ secrets.AWS_SECRET_ACCESS_KEY }}

%USERPROFILE%\\.aws\\config

> **You will need this in the steps below**

# Step 8 Add secrets to Github Secrets -- ensure you are in git hub repo on github

Enter your aws keys here so that github can communicate with AWS
![Picture17](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/cdd8d56b-de99-4f19-83f4-53320b020ff4)


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
![Picture23](https://github.com/muhammedtaufiq/3.13_assignment/assets/24953052/497e459a-8643-4728-bf5b-041c663bb0cc)


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

*parse-json:*

If parse-json: true and secret value is a valid stringified JSON object, it will be parsed and flattened. Each of the key value pairs in the flattened JSON object will become individual secrets. The original secret name will be used as a prefix.


# STEP 12-Push the changes to GitHub to start the workflow


*IMAGES*

# STEP 13-Adding logging to the serverless application

we add console.log to our index.js file to output this message "function was deployed successfully!!".
this message will be captured in *Cloudwatch* logs



