<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Monitoring with Flow Logs

**Project Link:** [View Project](http://nextwork.ai/projects/aws-networks-monitoring)

**Author:** dominiqueanderson@msn.com  
**Email:** dominiqueanderson@msn.com

---

## VPC Monitoring with Flow Logs

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_3e1e79a1)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a way to isolate your own cloud environment within the AWS cloud infrastructure. Its a useful way to create, monitor, and build applications through the vast array of AWS services.

### How I used Amazon VPC in this project

In today's project, we achieved two big milestones! First, we learned how to troubleshoot VPC peering connectivity issues. Second, we learned how to monitor VPC Flow Logs.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was unexpected error occurring with ping connectivity tests. This project really demonstrates the power of Route tables and how critical VPC peering connectivity is vital to ensure monitoring our Flow Logs can successfully happen.

### This project took me...

This project took me around 2,5 hours with demo time included.

---

## In the first part of my project...

### Step 1 - Set up VPCs

In this step, the goal is to emphasize and understand the importance of network connectivity and monitoring between two VPCs through VPC peering. 

### Step 2 - Launch EC2 instances

In this step, I'm creating EC2 instances in order to generate network traffic between our VPCs that can be monitored through CloudWatch 

### Step 3 - Set up Logs

I'm setting up VPC flow logs to monitor network traffic and create a storage space for the logs.

### Step 4 - Set IAM permissions for Logs

In this step, we need to create an IAM role for the VPC Flow Logs which will create the necessary permissions to initiate and record flow logs as well as publish them to our CloudWatch platform.

---

## Multi-VPC Architecture

I started my project by launching two VPCs each in one AZ with one public subnet in each VPC

The CIDR blocks for VPCs 1 and 2 have to be unique as the route tables within each VPC provides traffic the proper destination to a specific or grouping of IP addresses through CIDR blocks. If the CIDR blocks for each VPC are the same, the overlapping would cause a network connectivity error as traffic will be unable to reach the correct resource. Its like if there are two cities with the same street address and zip codes, how would the delivery driver know which house in which city is the correct one to go to.

### I also launched EC2 instances in each subnet

My EC2 instances' security groups allow SSH traffic and ICMP traffic. We need SSH traffic to be allowed in as this is the main way to connect to our EC2 instance over the internet. ICMP is needed for connectivity test

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_e7fa8775)

---

## Logs

Logs are like diary entries for computer systems that keep detailed records of network activity, resource usage, and other AWS services metrics to ensure your system is running properly. 

Log groups are like folders that contain all the log files. Usually, logs from the same source or application will go into the same log group, BUT logs are also region-specific. This means log data gets created and saved in the region it was created, although you can use CloudWatch dashboards to bring together logs from different regions.

### I also set up a flow log for VPC 1

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_e8398869)

---

## IAM Policy and Roles

I created an IAM policy because by default VPC Flow logs need permission to publish to the Amazon CloudWatch log group. By creating an IAM Role, we define a rule that allows policy holders e.g. our VPC Flow Log service the ability to create log streams and upload them into Cloud Watch

I also created an IAM role because services like VPC Flow Logs have to be associated with a role instead of JSON! Creating an IAM role will be necessary to give our VPC Flow Logs the access it needs to record and upload logs.

A custom trust policy is specific type of policy used for defining who/what is allowed access to the IAM role. 

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_4334d777)

---

## In the second part of my project...

### Step 5 - Ping testing and troubleshooting

We are sending network traffic from one VPC to another VPC. This is an important aspect of cloud networking / cloud engineering.

### Step 6 - Set up a peering connection

In this step, I will setup a peering connection so VPCs 1 and 2 can talk directly with each other 

### Step 7 - Analyze flow logs

In this step, we are tracking the network data that's been collected on our VPCs and then analyze that data to extract insights. 

---

## Connectivity troubleshooting

My first ping test between my EC2 instances had no replies, which means ICMP traffic could be blocked by security groups/ network ACLs. Perhaps the traffic is being routed to a wrong path.

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_99d4ba42)

I could receive ping replies if I ran the ping test using the other instance's public IP address, which means our second instance is actually allowing ICMP traffic.

---

## Connectivity troubleshooting

Looking at VPC 1's route table, I identified that the ping test with Instance 2's private address failed because we do not have a route in our VPC's route tables that directs traffic from one VPC to another. 

### To solve this, I set up a peering connection between my VPCs

I also updated both VPCs' route tables so that network traffic between the VPCs can be directed to the peering connection, which allows us to connect via the private IP addresses instead of only connecting via the public IP addresses through the internet!

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_7316a13d)

---

## Connectivity troubleshooting

I received ping replies from Instance 2's private IP address! This means network traffic is flowing properly through the private IP addresses of the VPCs as we created a peering connection that is added to the route table.

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_4ec7821f)

---

## Analyzing flow logs

Flow logs tell us about the source and destination of network traffic on which port and protocol as well as how much data was sent and if the response  / connection was successful.

For example, the flow log I've captured tells us that traffic went from 85.11.182.41 to 10.1.7.108. We can see the traffic was rejected.

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_d116818e)

---

## Logs Insights

Logs Insights is a special tool within Amazon CloudWatch that helps us with analyzing logs and creating visual graphs and charts through queries. 

I ran the query "Return the top 10 byte transfers by source and destination IP addresses" which analyzes the flow logs collected on EC2 Instance 1 and return the top 10 pairs of IP addresses based on the amount of data transferred between them.

![Image](http://nextwork.ai/vibrant_gold_swift_ox/uploads/aws-networks-monitoring_3e1e79a1)

---

---
