# VPC Monitoring with Flow Logs

A hands-on AWS networking project: setting up two peered VPCs, generating traffic between EC2 instances, and using VPC Flow Logs + CloudWatch Logs Insights to monitor and troubleshoot connectivity.

## What this project demonstrates
- Designing and connecting multi-VPC network architecture
- Diagnosing and resolving real connectivity failures (not just following steps — actually troubleshooting when something broke)
- Setting up IAM roles/policies to support AWS logging services
- Using CloudWatch Flow Logs and Logs Insights to analyze network traffic

## Architecture
Two VPCs, each with one public subnet in its own Availability Zone, each hosting an EC2 instance. The VPCs use non-overlapping CIDR blocks (required — overlapping ranges mean route tables can't tell which VPC a given IP address actually belongs to) and are connected via a VPC peering connection so instances can reach each other over private IP addresses instead of the public internet.

Security groups on both instances allow SSH (for remote access) and ICMP (for connectivity testing).

## Setup steps
1. **Launch two VPCs**, each with a public subnet, to set up the network foundation for peering
2. **Launch EC2 instances** in each VPC to generate traffic that can be monitored
3. **Set up VPC Flow Logs** to capture and store network traffic data
4. **Create an IAM role and custom trust policy** so the Flow Logs service has permission to publish logs to CloudWatch (Flow Logs must be associated with a role rather than a raw JSON policy)
5. **Establish a VPC peering connection** and update both route tables so traffic between VPCs routes through the peering connection
6. **Run connectivity tests and analyze the results** using CloudWatch Logs Insights

## Troubleshooting: the ping failure

The first ping test between the two EC2 instances, using private IP addresses, got no replies. Testing with the public IP addresses instead worked fine, confirming that ICMP wasn't blocked at the security group level — the problem was upstream in routing.

![Failed ping test](images/failed-ping-test.png)

Checking the route table for VPC 1 showed the real issue: there was no route directing traffic from one VPC to the other. Private-IP traffic had nowhere to go.

**Fix:** created a VPC peering connection between the two VPCs and added routes in both VPCs' route tables pointing to that peering connection.

![Route table with peering route](images/route-table-fix.png)

Re-running the ping test using private IPs returned replies — traffic was now flowing correctly over the peering connection.

![Successful ping test](images/successful-ping-test.png)

## Analyzing the logs

Flow Logs capture the source/destination IPs, ports, protocol, byte count, and accept/reject status of traffic. One captured entry showed traffic from `85.11.182.41` to `10.1.7.108` was rejected — useful for spotting blocked or misconfigured traffic.

Using CloudWatch Logs Insights, I ran a query to return the top 10 source/destination IP pairs by data transferred, which is a common pattern for identifying your heaviest traffic flows or spotting unexpected data movement.

## Key takeaway
This project reinforced how much of cloud networking troubleshooting comes down to isolating *where* in the stack a failure is happening — security group vs. route table vs. peering — rather than assuming the first likely cause. That diagnostic process is directly transferable to cloud support and on-call troubleshooting work.

## Time to complete
~2.5 hours, including analysis and documentation.

---
*Based on an AWS networking lab; architecture, troubleshooting, and write-up are my own.*

