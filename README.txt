# API Node.js with Serveless FrameWork in AWS

In this project we have an AWS cloud infrastructure with API Gateway, DynamoDB, AWS Lambda and AWS CloudFormation using the Serverless framework for development based on Infrastructure as a Code.

# Requirements

- AWS acount 
- NodeJS
- AWS CLI
- Microsoft Vscode

## Steps

### 1 - initial setup
- Create user in AWS IAM -> 
            download credentials -> 
                    "$ aws configure" in terminal and paste credentials

- Install the framework serverless:
    $ npm i -g serverless

- Run the commad "serverless" :
    $ serverless
    Login/Register: No
    Update: No
    Type: Node.js REST API
    Name: "project name"

- call directory previous created and open with microsoft Vscode

    $ cd "project name"
    $ code.

- open serverless.yml ->
            add region (region: us-east-1), on "provider:" scope ->
                    save and run "$ serverless deploy -v"


### 2 - Strucuture

- Create folder "src" ->
            move "handler.js" to "src" ->
                rename "handler.js" to "hello.js" ->
                    modify the code ->  const hello = async (event) => {module.exports = {handler:hello}} ->
                        update "serverless.yml"  handler: src/hello.handler

- Run  "$ serverless deploy -v"

### 3 - DynamoDB configure
- Update serverless.yml
       
        resources:
        Resources:
            ItemTable:
            Type: AWS::DynamoDB::Table
            Properties:
                TableName: ItemTable
                BillingMode: PAY_PER_REQUEST
                AttributeDefinitions:
                    - AttributeName: id
                    AttributeType: S
                KeySchema:
                    - AttributeName: id
                    KeyType: HASH


### 4 - Lambda Functions

- Get the "ARN" on DynamoDB AWS Console -> DynamoDB -> Overview -> Amazon Resource Name (ARN)

- update "serverless.yml" ->
            paste de follow code bellow "region:"

            iam:
                role:
                    statements:
                        - Effect: Allow
                        Action:
                            - dynamodb:PutItem
                            - dynamodb:UpdateItem
                            - dynamodb:GetItem
                            - dynamodb:Scan
                        Resource:
                            - arn:  "ARN HERE"


- install dependences: 
        npm init
        npm i uuid aws-sdk



- update "serverless.yml"

        functions:
        hello:
            handler: src/hello.handler
            events:
            - http:
                path: /
                method: get
        insertItem:
            handler: src/insertItem.handler
            events:
            - http:
                path: /item
                method: post
        fetchItems:
            handler: src/fetchItems.handler
            events:
            - http:
                path: /items
                method: get
        fetchItem:
            handler: src/fetchItem.handler
            events:
            - http:
                path: /items/{id}
                method: get
        updateItem:
            handler: src/updateItem.handler
            events:
            - http:
                path: /items/{id}
                method: put
