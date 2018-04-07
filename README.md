# AWSCodeDeploy
This project is create to do POC and demonstrate AWS code build Pipe line


1. Create IAM Roles 
CodeDeploy & EC2CodeDeploy 
2. Create EC2 instance with following categories 

     a. Choose AMI: Amazon Linux AMI 

     b. Choose Instance type: t2.micro 

     c. Configure Instance: Choose EC2CodeDeploy IAM role 

     d. Tag Instance: Name it what you please 

     e. Configure Security Group: 

         HTTP TCP 80 0.0.0.0/0 

         HTTP TCP 80 ::/0 

         SSH TCP 22 (YOUR IP ADDRESS) 

         HTTPS TCP 443 0.0.0.0/0 

         HTTPS TCP 443 ::/0 

     f. LAUNCH INSTANCE 

3. Login to EC2 instance 

4. Command line of Amazon Linux AMI 

     a. When server is booted 

          sudo su 

          yum -y update 

          yum install -y aws-cli 

          cd /home/ec2-user 

     b. Here you will setup your AWS access, secret, and region. 

          aws configure 

          aws s3 cp s3://aws-codedeploy-us-east-1/latest/install . --region us-east-1 (if in east AWS) 

          aws s3 cp s3://aws-codedeploy-us-west-2/latest/install . --region us-west-2 (if in west AWS) 

          chmod +x ./install 

     c. This is simply a quick hack to get the agent running faster. 

          sed -i "s/sleep(.*)/sleep(10)/" install 

          ./install auto 

     d. Verify it is running. 

          service codedeploy-agent status

		  
		  // Policy Role for Code Deploy
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "autoscaling:PutLifecycleHook",
        "autoscaling:DeleteLifecycleHook",
        "autoscaling:RecordLifecycleActionHeartbeat",
        "autoscaling:CompleteLifecycleAction",
        "autoscaling:DescribeAutoscalingGroups",
        "autoscaling:PutInstanceInStandby",
        "autoscaling:PutInstanceInService",
        "ec2:Describe*"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}

// Policy Trust for Code Deploy
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "codedeploy.<region>.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

// Instance Role for EC2 Instance
{ 
    "Version": "2012-10-17", 
    "Statement": [   
      {     
          "Action": [       
              "s3:Get*",       
              "s3:List*"     
          ],     
          "Effect": "Allow",     
          "Resource": "*"   
      } 
    ]
}