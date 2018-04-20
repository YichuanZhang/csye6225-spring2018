# CSYE6225-Spring2018 Cloud Computing Team

[![Build Status](https://travis-ci.com/YichuanZhang/csye6225-spring2018.svg?token=tKA3sSWpCQ9Gbyx5A8dJ&branch=master)](https://travis-ci.com/YichuanZhang/csye6225-spring2018)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/36cbec36e60049c39a1c9319714b8dee)](https://www.codacy.com/app/YichuanZhang/csye6225-spring2018?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=YichuanZhang/csye6225-spring2018&amp;utm_campaign=Badge_Grade)
<p>Team Member<p>

Yichuan Zhang  -  zhang.yichu@husky.neu.edu (Scrum master)

Xiao Li  -  li.xiao5@husky.neu.edu

Yang Yuan  -  yuan.yang@husky.neu.edu

# Web Cluster Structure
<img src="https://spring2018.csye6225.com/assignments/A09.png"></a>

Lambda function can be found here: <a href="https://github.com/YichuanZhang/csye6225-spring2018-lambda">Email Service Lambda</a>

# Instructions on Scripts and Structure of Server Cluster
	Make sure you have installed AWS Cli on machine and granted User access to it
	Script will take STACK_NAME as parameter
	Sequence of creating stacks should be network->cicd->application
# Scripts file path: /infrastructure/aws/cloudformation
<p>"csye6225-cf-networking.json"</p>
<ul>
	<li>The cloudFormation template for network stack</li>
</ul>
<p>"csye6225-cf-ci-cd.json"</p>
<ul>
	<li>The cloudFormation template for policy stack</li>
</ul>
<p>"csye6225-cf-application.json"</p>
<ul>
	<li>The cloudFormation template for application stack</li>
</ul>

## "csye6225-aws-cf-create-stack.sh" script will
<ul>
  <li>Create a cloudformation stack taking STACK_NAME as parameter</li>
	<li>Create and configure required networking resources</li>
	<li>Create a Virtual Private Cloud (VPC) resource called STACK_NAME-csye6225-vpc</li>
	<li>Create Internet Gateway resource called STACK_NAME-csye6225-InternetGateway</li>
	<li>Attach the Internet Gateway to STACK_NAME-csye6225-vpc VPC</li>
	<li>Create a public Route Table called STACK_NAME-csye6225-public-route-table</li>
	<li>Create a public route in STACK_NAME-csye6225-public-route-table route table with destination CIDR block 0.0.0.0/0 and STACK_NAME-csye6225-InternetGateway as the target</li>
</ul>

## "csye6225-aws-cf-create-ci-cd-stack.sh" script will
<ul>
	<li>Create "CodeDeployEC2S3" policy which allows EC2 instance put or get object from S3 bucket</li>
	<li>Create "TravisUploadToS3" policy which allows Travis CI deploy the application .WAR on S3 bucket</li>
	<li>Create "TravisCodeDeploy" policy which allows Travis CI call AWS CodeDeploy to deploy application</li>
	<li>Create "CloudWatchLogPolicy" policy which allows AWS Cloudwatch to get logs from EC2 instances</li>
	<li>Create "CodeDeployEC2ServiceRole" role for EC2</li>
	<li>Create "CodeDeployServiceRole" role for AWS CodeDeploy</li>
	<li>Create "LambdaExecutionRole" role for Lambda functions</li>
</ul>

## "csye6225-aws-cf-create-application-stack.sh" script will
<ul>
	<li>Create EC2 launch configuration with User-data</li>
	<li>Create an auto-scaling group with minimum of 3 instances and maximum of 7</li>
	<li>Create a load balancer which listen to Internet traffic and forward to EC2 instances</li>
	<li>Create ALB listeners listen to traffic on port 80 and 443</li>
	<li>Create a target group aiming at the auto-scaling group</li>
	<li>Create a type A resource record</li>
	<li>Create a DynamoDB to store tokens</li>
	<li>Create a RDS server to store POJO</li>
	<li>Create a S3 Bucket</li>
	<li>Create a SNS topic to notify Lambda function</li>
	<li>Create an auto-scaling policy based on EC2 instance's CPU usage</li>
</ul>	

## Termination stack scripts: 
	script should take STACK_NAME as parameter
	Sequence of termination stacks should be application->cicd->network
<ul>
	<li> "csye6225-aws-cf-terminate-stack.sh": Delete the stack and all networking resources.</li>
	<li> "csye6225-aws-cf-terminate-ci-cd-stack.sh": Delete the stack and all policy resources.</li>
	<li> "csye6225-aws-cf-terminate-application-stack.sh": Delete the stack and all application and server resources
</ul>

# Prerequisites for building and deploying web application on local machine
<ul>
  <li>Java 8</li>
  <li>Tomcat8</li>
  <li>MySQL</li>
</ul>

# Web Application
<p>The web application is implemented in:</p>
<ul>
  <li>Gradle build tool</li>
  <li>Spring boot application</li>
  <li>JPA repository model</li>
  <li>MySQL database as storage</li>
  <li>Spring security</li>
  <li> BCrypt password hashing scheme with salt</li>
  <li>Thymeleaf as server-side Java template engine</li>
  <li>Jmeter as load test</li>
  <li>Travis CI as continuous integration</li>
