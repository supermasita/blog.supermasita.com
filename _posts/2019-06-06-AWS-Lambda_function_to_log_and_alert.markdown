---
layout: post
title:  "AWS - Lambda function to log and alert"
date:   2019-06-06 07:04:16 -0300
tags: computers aws python lambda boto3
---
I ll send you POSTs - you keep track and let me know when something is broken, OK?

The idea is that you would have some code running somewhere, doing some stuff (very specific). Every now and then, you POST data to an **API Gateway** endpoint (not covered here), connected to **Lambda**. The Lambda function will save the data to S3 as a JSON and push to **SNS** and **Chime** if your POST reports a failure. 

Here is the example Lambda function:
```python
"""
NOTE!
 This is a simple working example, but **incomplete**. Error handling 
 is missing, formatting of alerts, return in functions, etc.

 You are expected to make POST to the API Gateway connected to this Lambda,
 sending at least 'id' and 'state' fields.
"""
import json
import boto3
import time
import urllib3

BUCKET = "xxxx"
SNS_ARN = "arn:aws:sns:xxxxx:xxxxxx:xxxxx"
CHIME_URL = "https://hooks.chime.aws/incomingwebhooks/xxxxxxxxxx"

s3 = boto3.client('s3')
sns = boto3.client('sns')

def chime_push(event):
    """ Send a POST to Chime's webhook with a 'Help!' and the event dump.
    """
    http = urllib3.PoolManager()
    query_str = json.dumps(event)
    encoded_data = json.dumps({"Content": "/md ## Help!!! \n {}".format(query_str) }).encode('utf-8')
    try:
        r = http.request('POST', CHIME_URL, body=encoded_data, 
                headers={'Content-Type': 'application/json'})
        return r.data
    except Exception as e:
        return e


def sns_push(event):
    """ Send a SNS (email) with the dump of the event
    """
    sns.publish(TargetArn=SNS_ARN,Message=json.dumps({'default': json.dumps(event)}),
        MessageStructure='json')


def s3_push(event):
    """ Save the event to a bucket as JSON. Add a field with timestamp.
    """
    ts = str(int(time.time()))
    event['ts'] = ts
    s3.put_object(
         Bucket=BUCKET,
         Key=f'executions/%s.json' % event['id'],
         Body=json.dumps(event),
    )


def lambda_handler(event, context):
    """ Your pushed event must contain 'id' and 'state'.
    """
    if 'id' not in event:
        return False        
    if event['state'] == 'FAILURE':
        sns_push()
        chime_push(event)
    return event
```

Example function to do the POST to API Gateway:
```python
def post_data(query):
    request = urllib2.Request(URL)
    request.add_header("Content-Type",'application/json')
    try:
        response = urllib2.urlopen(request, json.dumps(query))
        return response.read()
    except Exception as e:
        return e
```

### Keep reading
* <https://aws.amazon.com/lambda/>
* <https://aws.amazon.com/sns/>
* [Automating Chat Messages with Webhooks](https://docs.aws.amazon.com/chime/latest/ug/webhooks.html) 
* [Build an API Gateway API with Lambda Integration ](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-lambda-integration.html)
