# Create-and-configure-Codepipeline-with-Elastic-Beanstalk


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/badec152-2194-40de-b7b0-c016ceb02352)

Application name
**CodeDeployDemo
**

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/361b07bc-727c-423f-b81b-7347e0719e0f)

Under Platform type in Platform: Choose Node.js

Keep Platform Branch and Version By Default

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/e5c57006-fa79-4b63-adc2-1a60d9cbdeae)


Configure service access :

Service role : Choose Create and use new service role
EC2 Instance profile: Choose task204_ec2_<RANDOM_NUMBER>. This task will already be created for you.
Click on Next button.

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/e69f973e-803e-481d-9319-9df73c61193b)


```{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "BucketAccess",
			"Action": [
				"s3:Get*",
				"s3:List*",
				"s3:PutObject"
			],
			"Effect": "Allow",
			"Resource": [
				"arn:aws:s3:::elasticbeanstalk-*",
				"arn:aws:s3:::elasticbeanstalk-*/*"
			]
		},
		{
			"Sid": "XRayAccess",
			"Action": [
				"xray:PutTraceSegments",
				"xray:PutTelemetryRecords",
				"xray:GetSamplingRules",
				"xray:GetSamplingTargets",
				"xray:GetSamplingStatisticSummaries"
			],
			"Effect": "Allow",
			"Resource": "*"
		},
		{
			"Sid": "CloudWatchLogsAccess",
			"Action": [
				"logs:PutLogEvents",
				"logs:CreateLogStream",
				"logs:DescribeLogStreams",
				"logs:DescribeLogGroups"
			],
			"Effect": "Allow",
			"Resource": [
				"arn:aws:logs:*:*:log-group:/aws/elasticbeanstalk*"
			]
		},
		{
			"Sid": "ElasticBeanstalkHealthAccess",
			"Action": [
				"elasticbeanstalk:PutInstanceStatistics"
			],
			"Effect": "Allow",
			"Resource": [
				"arn:aws:elasticbeanstalk:*:*:application/*",
				"arn:aws:elasticbeanstalk:*:*:environment/*"
			]
		},
		{
			"Sid": "MetricsAccess",
			"Action": [
				"cloudwatch:PutMetricData"
			],
			"Effect": "Allow",
			"Resource": "*"
		},
		{
			"Sid": "QueueAccess",
			"Action": [
				"sqs:ChangeMessageVisibility",
				"sqs:DeleteMessage",
				"sqs:ReceiveMessage",
				"sqs:SendMessage"
			],
			"Effect": "Allow",
			"Resource": "*"
		},
		{
			"Sid": "DynamoPeriodicTasks",
			"Action": [
				"dynamodb:BatchGetItem",
				"dynamodb:BatchWriteItem",
				"dynamodb:DeleteItem",
				"dynamodb:GetItem",
				"dynamodb:PutItem",
				"dynamodb:Query",
				"dynamodb:Scan",
				"dynamodb:UpdateItem"
			],
			"Effect": "Allow",
			"Resource": [
				"arn:aws:dynamodb:*:*:table/*-stack-AWSEBWorkerCronLeaderRegistry*"
			]
		},
		{
			"Sid": "ECSAccess",
			"Effect": "Allow",
			"Action": [
				"ecs:Poll",
				"ecs:StartTask",
				"ecs:StopTask",
				"ecs:DiscoverPollEndpoint",
				"ecs:StartTelemetrySession",
				"ecs:RegisterContainerInstance",
				"ecs:DeregisterContainerInstance",
				"ecs:DescribeContainerInstances",
				"ecs:Submit*",
				"ecs:DescribeTasks"
			],
			"Resource": "*"
		},
		{
			"Sid": "AllowECSTagResource",
			"Effect": "Allow",
			"Action": [
				"ecs:TagResource"
			],
			"Resource": "*",
			"Condition": {
				"StringEquals": {
					"ecs:CreateAction": [
						"RegisterContainerInstance",
						"StartTask"
					]
				}
			}
		}
	]
}
```





![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/904ee274-b8fe-4fc4-9751-868c6452316f)


Choose Default VPC

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/e76e1ee9-a72f-4e28-8728-1ee1271cd989)

Choose  us-east-1a and us-east-1b under Instance subnets.

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/df153366-fc02-415e-b56b-e18dc9fdd38a)


Scroll-down and Under Capacity:

Environment Type: Select Load balanced

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/787a91cd-a586-4ccb-92a4-ecbb4fbdc275)


Under Instance type : Select t2.micro and remove the existing instance types. 

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/e6ba1166-5c44-4728-80dd-4c249549dce9)

Under Listeners: Select the Listener that is already present.

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/80268cd8-b8c6-4881-b921-f61d79d517c8)


Under Configure updates, monitoring, and logging - optional

Select Basic under System in the Monitoring section.


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/dcae5be2-9d61-426c-b344-8ff94f18141d)


Under Managed platform updates:

Uncheck the Activated checkbox under Managed updates.

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/38ebf316-2acb-4554-8b43-86b9b1f7dd6d)


 Leave everything as default and click on Next button. 

Click on Submit button. The app and environment will start to create.  Note: This process usually takes about 10 to 15 minutes to complete.


### Task 3 Create a S3 bucket

Go to S3, click on New Bucket
Name the bucket as DemoCodeDeployBucket2024

Upload the nodejs-v2-blue.zip file in the bucket.

### Task 4.  Enable versioning for the S3 bucket and copy the object key

Open the bucket with the name DemoCodeDeployBucket2024.

Under Properties, Go to Bucket Versioning 

Click on Enable Bucket Versioning


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/f1784780-7f26-4038-9f7e-d3f58eac73f0)


Versioning is now enabled.

Let's copy the object key for the zip file present there, to do so, click on the Objects tab.

Click on the object present to see its properties.
Copy the object Key, and save it to notepad. We'll need this in further steps.

nodejs-v2-blue.zip

### Task 5 Create a CodePipeline pipeline
Navigate to CodePipeline by clicking on the Services menu at the top, then click on CodePipeline in the Developer Tools section.

On the homepage of CodePipeline, click on the Create pipeline button.

DemoPipeline

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/1b9e55d4-9a9f-475a-9297-15abcefb5473)

Create a new Service Role 


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/3ce71795-647b-4f1b-9afe-9a434d3d8ba8)


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/73ed8713-d6a7-4cdc-806e-25a1f537362d)


S3 object key: Enter nodejs-v2-blue.zip

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/74186c10-a222-4ba8-8f77-7ea6b9b6bcb3)

Skip Buildstage

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/9fb5e886-24a0-429f-9293-a95dad63255e)

For the Step-4, Add deploy stage

Deploy provider: Enter AWS Elastic Beanstalk

Region: US East (N. Virginia)

Application name: Enter CodeDeployDemo

Environment name: Enter WhizDemo-env

Click on the Next button.

![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/5d314589-333e-470d-90ae-fffdcc76fac7)


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/d04945db-c16f-4118-8b83-28f784495a09)


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/eb5212ff-e47b-499c-9c58-4b1c7533277f)


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/f74514e1-7921-4237-9d76-0cb9a732ec8d)


![image](https://github.com/Asma09Akram/Create-and-configure-Codepipeline-with-Elastic-Beanstalk/assets/124654068/f7882f0d-d023-4856-8598-0edd115c6f9f)


