---
layout: post
title:  "AWS - Download all objects from a S3 bucket"
date:   2019-01-21 07:04:16 -0300
tags: aws s3 python computers 
---
Boto's [list_object_v2()](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html#S3.Client.list_objects_v2) is limited to 1000 objects at a time.

My approach was to use a function similar to the following, which worked perfectly to download around 10K objects (but if you need to download millions of objects, you will need to improve it):

```python
def download_site_configs_s3(key_id, key_secret, bucket, download_path):
    """ List all objects using 'IsTruncated' for pagination and add them to a list.
        Iterate the list and download the objects.
    """
    client = boto3.client('s3', aws_access_key_id=key_id,
                          aws_secret_access_key=key_secret)
    s3_bucket_response = client.list_objects_v2(Bucket=bucket)
    s3_content_list = s3_bucket_response['Contents']
    object_list = []
    while True:
        for s3object in s3_content_list:
            object_list.append(s3object['Key'])
        if s3_bucket_response['IsTruncated'] is not True:
            break
        else:
            s3_bucket_response = client.list_objects_v2(
                Bucket=bucket, ContinuationToken=s3_bucket_response['NextContinuationToken'])
            s3_content_list = s3_bucket_response['Contents']

    for object_key in object_list:
        client.download_file(bucket, object_key, '%s/%s' %
                             (download_path, object_key))
```

That's it!

