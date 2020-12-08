## AWS SAM Github Action
This action supports the following capabalities:
1. Run integration tests against local Lambda endpoint(s). You can learn more about integrated with automated tests on [AWS](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-using-automated-tests.html)
2. Build, package and deploy an AWS SAM application

### Run integration tests
In addition, to the required environment variables, set the following environment variables in your job:
- **AWS_LOCAL_START_LAMBDA**: Set it to any value
- **PYTHON_TEST_DIR**: Set it to the name of the test directory which has integration tests you want to run the app against. As an example, the value for the following project will be `tests/integration/dev/`
```
.
|-- hello_world
|   +-- app.py
|   +-- requirements.txt
|-- tests
|   +-- integration
|       +-- dev
|           +-- test_handler.py
|       +-- prod
|           +-- test_handler.py
|-- template.yaml
|-- README.md
```

Use the following workflow to invoke Lambda function(s) locally
```
name: Example CD workflow
on:
  push:
    branches: [ master ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: sam build
      uses: poketapp/gh-action-aws-sam@v1
      env:
        AWS_REGION: 'ca-central-1'
        AWS_ACCESS_KEY_ID: ${{secrets.AWS_ACCESS_KEY_ID}}
        AWS_SECRET_ACCESS_KEY: ${{secrets.AWS_SECRET_ACCESS_KEY}}
        AWS_LOCAL_START_LAMBDA: 'True'
        PYTHON_TEST_DIR: 'tests/integration/dev'
```

### Build, package and deploy an AWS SAM application
In addition, to the required environment variables, set the following environment variables in your job:
- **AWS_DEPLOY_BUCKET**: This is the name of the S3 bucket which will contain the deployment package
- **AWS_STACK_NAME**: This is the name of your CloudFormation stck

Use the following workflow to build, package and deploy an AWS SAM application
```
name: Example CD workflow
on:
  push:
    branches: [ master ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: sam build
      uses: poketapp/gh-action-aws-sam@v2
      env:
        AWS_REGION: 'eu-west-2'
        AWS_ACCESS_KEY_ID: ${{secrets.AWS_ACCESS_KEY_ID}}
        AWS_SECRET_ACCESS_KEY: ${{secrets.AWS_SECRET_ACCESS_KEY}}
        AWS_DEPLOY_BUCKET: 'test-s3-bucket'
        AWS_STACK_NAME: 'Test-Stack'
```

It is recommended to store access key id and secret access key in Github secrets.