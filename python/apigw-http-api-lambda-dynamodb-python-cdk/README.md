
# AWS API Gateway HTTP API to AWS Lambda in VPC to DynamoDB CDK Python Sample!


## Overview

Creates an [AWS Lambda](https://aws.amazon.com/lambda/) function writing to [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) and invoked by [Amazon API Gateway](https://aws.amazon.com/api-gateway/) REST API. 

![architecture](docs/architecture.png)

## API Authentication and Rate Limiting

This stack implements AWS Well-Architected Framework best practice **REL05-BP02: Throttle requests** using API Gateway usage plans and API keys for per-client rate limiting.

### Usage Plan Configuration
- **Rate Limit:** 10 requests/second per API key
- **Burst Limit:** 20 requests per API key
- **Daily Quota:** 10,000 requests per API key

### API Key Requirement
All API requests must include a valid API key in the `x-api-key` header. Without a valid key, requests return `403 Forbidden`.

### Retrieving API Key Value
After deployment, retrieve the API key value:
1. Note the API Key ID from the stack outputs
2. Navigate to AWS API Gateway console → API Keys
3. Click "Show" to reveal the API key value

### Using the API
Include the API key in all requests:
```bash
curl -X POST https://your-api-url/prod \
  -H "x-api-key: your-api-key-value" \
  -H "Content-Type: application/json" \
  -d '{"year":"2023","title":"test","id":"123"}'
```

### Benefits
- Per-client rate limiting prevents individual consumers from exhausting resources
- Quota management enables fair usage across multiple API consumers
- API key authentication provides basic access control
- High-volume consumers can be identified and managed separately

## Setup

The `cdk.json` file tells the CDK Toolkit how to execute your app.

This project is set up like a standard Python project.  The initialization
process also creates a virtualenv within this project, stored under the `.venv`
directory.  To create the virtualenv it assumes that there is a `python3`
(or `python` for Windows) executable in your path with access to the `venv`
package. If for any reason the automatic creation of the virtualenv fails,
you can create the virtualenv manually.

To manually create a virtualenv on MacOS and Linux:

```
$ python3 -m venv .venv
```

After the init process completes and the virtualenv is created, you can use the following
step to activate your virtualenv.

```
$ source .venv/bin/activate
```

If you are a Windows platform, you would activate the virtualenv like this:

```
% .venv\Scripts\activate.bat
```

Once the virtualenv is activated, you can install the required dependencies.

```
$ pip install -r requirements.txt
```

At this point you can now synthesize the CloudFormation template for this code.

```
$ cdk synth
```

To add additional dependencies, for example other CDK libraries, just add
them to your `setup.py` file and rerun the `pip install -r requirements.txt`
command.

## Deploy
At this point you can deploy the stack. 

Using the default profile

```
$ cdk deploy
```

With specific profile

```
$ cdk deploy --profile test
```

## After Deploy
Navigate to AWS API Gateway console and test the API with below sample data (include your API key):
```json
{
    "year":"2023", 
    "title":"kkkg",
    "id":"12"
}
```

You should get below response 

```json
{"message": "Successfully inserted data!"}
```

## Cleanup 
Run below script to delete AWS resources created by this sample stack.
```
cdk destroy
```

## Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

Enjoy!
