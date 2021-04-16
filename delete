from __future__ import print_function
import boto3
import time, urllib
import json
"""Code snippet for deleting the objects from AWS S3 bucket as soon as objects uploaded on S3 bucket
@author: Prabhakar G
"""
print ("*"*80)
print ("Initializing..")
print ("*"*80)

s3 = boto3.client('s3')
sns_client = boto3.client('sns')

def lambda_handler(event, context):
    # TODO implement
    #for record in event['query']['Records']
    print('hello major')
    print(event)
    message = event['Records'][0]['Sns']['Message']
    obj = json.loads(message)
    print("From SNS: " + message)
    #sns_client.publish(TopicArn='arn:aws:sns:us-east-1:560675490114:snstopic',Message='the file is deleting')

    bucket = obj['Records'][0]['s3']['bucket']['name']
    object_key = urllib.unquote_plus(obj['Records'][0]['s3']['object']['key'])
    
    try:
        print ("Using waiter to waiting for object to persist through s3 service")
        # It will wait till s3 service return the output
        waiter = s3.get_waiter('object_exists')
        waiter.wait(Bucket=bucket, Key=object_key)
        response = s3.head_object(Bucket=bucket, Key=object_key)
        print(event)
        print ("Key :"+str(object_key))
        print ("Content Type : "+str(response['ContentType']))
        print ('ETag :' +str(response['ETag']))
        print ('Content-Length :'+str(response['ContentLength']))
        print ('KeyName :'+str(object_key))
        print ('Deleting object :'+str(object_key))
        # Delete the uploaded objects/ data from defined bucket
        time.sleep(5)
        s3.delete_object(Bucket=bucket, Key=object_key)
        return response['ContentType']
    except Exception as err:
        print ("Error -"+str(err))
        return err
