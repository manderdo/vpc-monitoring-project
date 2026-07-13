# VPC Monitoring with Flow Logs

## Description
This project focuses on cloud networking and monitoring by establishing connectivity between two VPCs. It includes setting up VPC peering, troubleshooting connectivity issues using route tables, and monitoring network traffic using Amazon VPC Flow Logs and CloudWatch Logs Insights.

## Project Overview
- **Duration:** Approximately 2.5 hours.
- **Goal:** Learn to troubleshoot VPC peering connectivity and monitor network traffic via Flow Logs.

## Key Steps
1.  **VPC Architecture:** Launched two VPCs, each with a public subnet in one Availability Zone (AZ).
2.  **EC2 Setup:** Deployed EC2 instances in each subnet, configured with security groups allowing SSH and ICMP traffic[cite: 1].
3.  **Logging:** Configured VPC Flow Logs to monitor traffic and created an IAM role with the necessary permissions to publish logs to Amazon CloudWatch[cite: 1].
4.  **Connectivity:** Troubleshooting connectivity by identifying route table limitations and establishing a VPC peering connection to allow private communication between instances[cite: 1].
5.  **Analysis:** Analyzed captured network data using CloudWatch Logs Insights to extract traffic insights, such as identifying top byte transfers[cite: 1].

## Lessons Learned
- **CIDR Blocks:** Unique CIDR blocks are essential for VPCs to avoid overlapping, which causes connectivity errors[cite: 1].
- **Troubleshooting:** Ping tests helped identify that private IP communication requires properly configured route tables directing traffic to the peering connection[cite: 1].
- **Monitoring:** CloudWatch Logs Insights is a powerful tool for querying and visualizing log data to understand network traffic patterns[cite: 1].
