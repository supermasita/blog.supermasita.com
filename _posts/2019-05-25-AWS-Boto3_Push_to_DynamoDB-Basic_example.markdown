---
layout: post
title:  "Boto3 - Push to DynamoDB - Basic example"
date:   2019-05-25 07:04:16 -0300
tags: computers aws dynamodb python boto3 
---
A super basic example to get you started pushing things to DynamoDB.

Read your AWS credentials from [OS enviroment variables](https://docs.python.org/3/library/os.html#os.environ) and push an items to the DynamoDB tables as key:value.

```python
import boto3
import os

KEY="test_key"
VALUE="test_value"
YOUR_TABLE="test_table"

# Read your credentials from OS environment variables
ACCESS_KEY = os.environ['AWS_ACCESS_KEY_ID']
SECRET_KEY = os.environ['AWS_SECRET_ACCESS_KEY']

# Create client
client = boto3.resource(
    'dynamodb',
    aws_access_key_id=ACCESS_KEY,
    aws_secret_access_key=SECRET_KEY,
)

# Push content to table
table = client.Table(YOUR_TABLE)
table.put_item(TableName=YOUR_TABLE, Item={KEY: VALUE})
```

### Keep reading
* [Python OS.ENVIRON](https://docs.python.org/3/library/os.html#os.environ)
* [Boto3 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) (Need to push many items? Check "[batch_write_item](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/dynamodb.html?highlight=dynamodb#DynamoDB.ServiceResource.batch_write_item)")
