

## Scheduling Starting and Stopping instances lambda function
## Overview
This Function runs on a cloudwatch schedule, performing stop/start actions on instances based on TIME tags applied.
Instances to be applied the function should have a TIME tagg as followes:
```
  Key: TIME
  Value: 1200///0900

# Where 1200 and 0900 are stop and start utc time respectively.  
```

## How to use this fuction
### A. Create a role with an inline policy.
Create a role with an inline policy that will allow the function to start and stop instances, describe regions and tag information as follows:
 ```
 {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:DescribeRegions",
                "ec2:DescribeTags"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}  
```
### B. Configure the function on Lamda console
1. Download the function [lambda_function.zip](https://github.com/ericsoi/Schedule-start-stop-of-AWS-ec2-instances/blob/master/lambda_function/lambda_function.py)
2. Navigate to [AWS lambda console](https://console.aws.amazon.com/lambda/)
3. Select **create function**
4. Under **Name**, enter the name of your function
5. Under **Runtime**, select **Python3.6**
6. **Choose an existing role** under **role**
7. Choose a role acreated in **A** above.
8. Select **Create Function**
9. Under **Function code**, select **upload zip file** and upload the downloaded zipfile. 
10. Increase **Timeout option** under **Basic settings**
11. Select **save** then **test**

### C. Create a schedule on cloudwatch
1. Navigate to [AWS lambda cloudwatch](https://console.aws.amazon.com/cloudwatch)
2. Create a rule with a one hour schedule Cron expression(00 * * * ? *).
3. Under **Targets**, select the Lambda Function created on section A.
4. Select **Configure details** and enter the name of the rule
5. Select **Create rule**
