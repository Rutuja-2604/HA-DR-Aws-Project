
**<h2>Setup Guide – High Availability & Disaster Recovery on AWS</h2>**

**<h2>🏗️ Step 1: Create VPCs in Two Regions</h2>**

Go to VPC Dashboard.

Create VPC-1 in Region A (e.g., ap-south-1 – Mumbai).

    CIDR block: 10.0.0.0/16

    Create 2 public subnets in different AZs.

Create VPC-2 in Region B (e.g., us-east-1 – N. Virginia).

    CIDR block: 20.0.0.0/16
    
    Create 2 public subnets in different AZs.
    


**<h2>⚙️ Step 2: Launch EC2 Instances & Auto Scaling</h2>**

1.Go to EC2 Dashboard in Region A.

![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/ec2.PNG?raw=true)


![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/ec2%20v.PNG?raw=true)

2.Create a Launch Template with:
 Amazon Linux 2 / Ubuntu AMI
 
  Instance type: t2.micro (for testing)
  
Security group: allow HTTP (80), HTTPS (443), SSH (22).

 User Data (optional for web app):

#!/bin/bash

yum install -y httpd

systemctl start httpd

systemctl enable httpd

echo "Hello from Region A" > /var/www/html/index.html

![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/l1.PNG?raw=true)

![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/template%20v.PNG?raw=true)

3.Create an Auto Scaling Group (ASG) using this launch template.

    Attach ASG to 2 subnets in Region A.

    Min size = 2, Desired = 2, Max = 4.

4.Repeat steps in Region B, but update User Data:

     echo "Hello from Region B" > /var/www/html/index.html
   ![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/v1.PNG?raw=true)

   ![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/auto%20v.PNG?raw=true)

**<h2>🌐 Step 3: Setup Target Group and Load Balancers</h2>**

1.In Region A, create an Target Group.

     select EC2 instances that you want to target.

     
![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/target%20group.PNG?raw=true)

![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/target%20v.PNG?raw=true)

2.In Region A, create an **Application Load Balancer (ALB).**

      Attach ALB to the 2 subnets in different AZs.

3.Target Group → attach the ASG.

       Listener → HTTP (80) forward to Target Group.

4.Repeat the same steps in Region B.

![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/load.PNG?raw=true)

![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/load%20v.PNG?raw=true)

**<h2>🌍 Step 4: Configure Route 53 for Failover</h2>**

1.Go to Route 53 → Hosted Zones.

2.Create a new hosted zone (e.g., myhaapp.com).

3.Add 2 A-records with failover policy:

      Record 1 (Primary) → Alias → ALB DNS name (Region A).

      Record 2 (Secondary) → Alias → ALB DNS name (Region B).

       Set health check for Region A load balancer.

4.Test DNS:

      If Region A is UP → traffic goes to Region A.

      If Region A is DOWN → Route 53 sends traffic to Region B.

![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/route53.PNG?raw=true)


![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/route%2053%20v.PNG?raw=true)

**<h2>🔍 Step 5: Testing Disaster Recovery</h2>**

  Stop all EC2 instances in Region A → Route 53 will failover to Region B.

  Restart Region A instances → traffic will return to primary region.

 ![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/domain.PNG?raw=true)

 
 ![Alt text](https://github.com/Rutuja-2604/HA-DR-Aws-Project/blob/main/Images/domain%20v.PNG?raw=true)
 

**<h2>📊 Monitoring & Logging</h2>**

Enable **CloudWatch Alarms** for CPU, Memory, and Instance Health.

Enable **ALB** Access Logs to S3.

Setup **SNS** Alerts for failures.


**<h2>✅ Final Architecture</h2>**

2 VPCs across 2 AWS regions.

Auto Scaling Groups with 2 EC2 instances each.

Load Balancer per region.

Route 53 DNS Failover between regions.
