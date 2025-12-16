**<h2>High Availibility & Disaster Recovery Management On AWS:</h2>**

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/85c56a1f-8797-4991-b9e6-a6f27a788314" />





**<h2>Project Overview:</h2>**

“This project designs a highly available and disaster-resilient web application on AWS using VPC, ALB, Auto Scaling, RDS Multi-AZ, S3 replication, and Route 53 failover. It ensures continuous uptime, automatic failover, data protection, and quick recovery across multiple Availability Zones and regions with minimal downtime.”

**<h2>🏗️ Architecture:</h2>**

The architecture consists of:

Two VPCs in different AWS Regions (for DR & fault isolation).
Auto Scaling Groups (ASG) in each region for scaling EC2 instances automatically.
Two EC2 Instances per region to ensure redundancy within a region.
Elastic Load Balancer (ALB/ELB) in each region to distribute traffic.
Amazon Route 53 configured with DNS failover to route traffic between regions.

**<h2>📌 Flow:</h2>**

Route 53 routes traffic to the nearest healthy region.
Load Balancers distribute requests to EC2 instances.
Auto Scaling adjusts the number of instances based on demand.
If one region goes down, Route 53 automatically redirects traffic to the other region.
⚙️ AWS Services Used
VPC (Virtual Private Cloud) – Isolated networking environment.
EC2 (Elastic Compute Cloud) – Web/app server instances.
Auto Scaling – Automatic scaling of EC2 instances.
Elastic Load Balancer (ELB/ALB) – Traffic distribution.
Route 53 – DNS service for failover and disaster recovery.
S3 (optional) – Data backup and static content hosting.
CloudWatch – Monitoring and alerts.

**<h2>🚀 Deployment Steps:</h2>**

Create VPCs in two different AWS regions.

Launch Auto Scaling Groups in each VPC with EC2 instances.

Attach Load Balancers to distribute traffic across instances.

Configure Route 53 Failover Policy:

Primary: Region 1 Load Balancer
Secondary: Region 2 Load Balancer
Test Failover: Stop instances or simulate failure to check Route 53 failover.


**<h2>📊 High Availability & Disaster Recovery Benefits:</h2>**

Fault Tolerance: Even if one instance fails, others in the ASG handle traffic.
Regional Redundancy: If an entire AWS region fails, traffic is redirected to another.
Scalability: ASG ensures resources adjust based on demand.
Business Continuity: Ensures uptime and availability during disasters.

