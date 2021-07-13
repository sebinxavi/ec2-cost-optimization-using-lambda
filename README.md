# EC2 Cost Optimization Using AWS Lambda Function

Using the lambda function with python code, we will be able to shut down the EC2-instances automatically.  The python code that can be executed automatically using the aws cloudwatch rules is provided below. 

## General Information

To save costs on AWS EC2, it may be necessary in some cases to automatically shut down EC2 instances during off-peak hours. For example, if the developers have a few EC2 instances running, we can automatically shut them down during their non-working hours using lambda functions and cloudwatch rules.

## Technology Used
- AWS Lambda
- AWS Cloudwatch

## About the python code

The python codes added below will stop and start all ec2-instances every day at 12:15 AM GMT and 08:15 AM GMT respectively with instance tags key: "env" and value: "testing"  

The python code has been written below::

### 1. To Stop the Instance
~~~
import boto3

REGION = "ap-south-1"

def lambda_handler(event, context):
    
  ec2 = boto3.client('ec2',region_name=REGION)

  all_ec2 = ec2.describe_instances(
      
      Filters=[
        {'Name':'tag:env', 'Values':["testing"]}
      ]
  )

  for instance in all_ec2['Reservations'][0]['Instances']:
    
    print("Stopping Ec2 : {} ".format( instance['InstanceId'] ))
    
    ec2.stop_instances(InstanceIds=[ instance['InstanceId'] ])
~~~

### 2. To Start the Instance

~~~
import boto3

REGION = "ap-south-1"

def lambda_handler(event, context):
    
  ec2 = boto3.client('ec2',region_name=REGION)

  all_ec2 = ec2.describe_instances(
      
      Filters=[
        {'Name':'tag:env', 'Values':["testing"]}
      ]
  )

  for instance in all_ec2['Reservations'][0]['Instances']:
    
    print("Starting Ec2 : {} ".format( instance['InstanceId'] ))
    
    ec2.start_instances(InstanceIds=[ instance['InstanceId'] ])
~~~

## Steps to be done::

### 1. Create an IAM role granting EC2 Full access to Lambda
##### Select the AWS Service - Lambda
#
![alt text](https://i.ibb.co/TqQhvWn/role1.png)
##### Grant EC2 Full access to Lambda
#
![alt text](https://i.ibb.co/5hsHkxb/role2.png)

### 2. Create Lambda Function with both the start and stop EC2 Python Codes
##### Create the Lambda Function
#
![alt text](https://i.ibb.co/7Vyxjdn/lambda1.png)
##### Add the python code to stop/start the instance in separate lambda functions
#
![alt text](https://i.ibb.co/s5L82rR/lambda2.png)
##### Check the added lambda functions
# 
![alt text](https://i.ibb.co/xJ7gMPW/lambda3.png)

### 3. Create the cloudwatch rules to automatically run the lambda function 

##### Cloudwatch rule to stop the ec2-instances at 12:15 AM GMT every day
![alt text](https://i.ibb.co/wcm3XB9/cloudwatch1.png)

#
![alt text](https://i.ibb.co/2k5tCvP/cloudwatch2.png)


##### Cloudwatch rule to start the ec2-instances at 08:15 AM GMT every day

![alt text](https://i.ibb.co/brHSMv9/cloudwatch3.png)
#
![alt text](https://i.ibb.co/frwHTxJ/cloudwatch4.png)

##### Check the Cloudwatch rules
![alt text](https://i.ibb.co/q9X6kHg/cloudwatch5.png)
#
#
## The Cloudwatch rules will execute the python codes added in the Lambda functions and it will stop and start all ec2-instances every day at 12:15 AM GMT and 08:15 AM GMT respectively with the instance tags key: "env" and value: "testing"  
#
## Author
Created by [@sebinxavi](https://www.linkedin.com/in/sebinxavi/) - feel free to contact me and advise as necessary!

<a href="mailto:sebin.xavi1@gmail.com"><img src="https://img.shields.io/badge/-sebin.xavi1@gmail.com-D14836?style=flat&logo=Gmail&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/sebinxavi"><img src="https://img.shields.io/badge/-Linkedin-blue"/></a>
