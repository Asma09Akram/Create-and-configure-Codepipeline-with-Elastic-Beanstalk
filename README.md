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
}```

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
