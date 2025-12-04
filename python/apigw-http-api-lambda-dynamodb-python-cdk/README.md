
# AWS API Gateway HTTP API to AWS Lambda in VPC to DynamoDB CDK Python Sample!


## Overview

Creates an [AWS Lambda](https://aws.amazon.com/lambda/) function writing to [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) and invoked by [Amazon API Gateway](https://aws.amazon.com/api-gateway/) REST API. 

![architecture](docs/architecture.png)

## Capacity Planning and Throttling

This section documents capacity planning and throttling configuration per AWS Well-Architected Framework best practice **REL05-BP02: Throttle requests**.

### Load Testing Requirements

Before deploying to production, conduct load testing to establish appropriate throttling limits:

**Testing Methodology:**
1. Use load testing tools (e.g., Apache JMeter, Gatling, AWS Distributed Load Testing)
2. Test with realistic payload sizes and request patterns
3. Gradually increase load to identify breaking points
4. Monitor all components: API Gateway, Lambda, DynamoDB
5. Document maximum sustainable throughput

**Metrics to Capture:**
- Maximum sustained requests per second
- Burst capacity (short-term spike handling)
- Average response time under load
- P99 response time
- Lambda concurrent executions
- DynamoDB consumed write capacity units
- Error rates at various load levels

### Recommended Throttle Limits

**Note:** The values below are placeholders. Replace with actual tested limits for your workload.

**API Gateway Stage Throttle:**
- Rate Limit: TBD requests/second (based on load testing)
- Burst Limit: TBD requests (based on load testing)

**Usage Plan Throttle (per API key):**
- Rate Limit: TBD requests/second per client
- Burst Limit: TBD requests per client
- Daily Quota: TBD requests per client

**WAF Rate Limit:**
- TBD requests per 5 minutes per IP address

**Lambda Reserved Concurrency:**
- TBD concurrent executions (based on load testing)

### Current Resource Configuration

**Lambda Function:**
- Memory: 1024 MB
- Timeout: 5 minutes
- Runtime: Python 3.9

**DynamoDB Table:**
- Billing Mode: On-demand (auto-scaling)
- Point-in-time Recovery: Enabled

**VPC Configuration:**
- CIDR: 10.1.0.0/16
- Subnet: Private isolated (/24)

### Increasing Limits

Before increasing any throttle limits:

1. **Verify Current Capacity:** Conduct load testing at current limits
2. **Scale Backend Resources:** Ensure Lambda and DynamoDB can handle increased load
3. **Test New Limits:** Load test at proposed new limits
4. **Document Results:** Update this README with tested capacity
5. **Monitor After Changes:** Watch CloudWatch metrics for issues
6. **Gradual Increases:** Increase limits incrementally, not all at once

### Monitoring

Monitor these CloudWatch metrics:

**API Gateway:**
- Count (total requests)
- 4XXError, 5XXError
- Latency, IntegrationLatency

**Lambda:**
- Invocations
- Errors, Throttles
- Duration
- ConcurrentExecutions

**DynamoDB:**
- ConsumedWriteCapacityUnits
- UserErrors, SystemErrors
- SuccessfulRequestLatency

**WAF:**
- BlockedRequests
- AllowedRequests

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
Navigate to AWS API Gateway console and test the API with below sample data 
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
