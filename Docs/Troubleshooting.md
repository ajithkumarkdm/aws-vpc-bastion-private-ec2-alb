# AWS VPC Project — Troubleshooting Guide

## 1. ALB Target Health: Connection Timeout

**Problem:** The ALB target group reported a connection timeout for the private EC2 instance.

**Checks performed:**
- Verified that the EC2 instance was running.
- Checked that Nginx was running.
- Tested the backend HTTP response.
- Reviewed the private EC2 security group rules.

**Resolution:** Added an inbound HTTP rule for port 80 to the private EC2 security group, using the ALB security group as the source. Retained the required Bastion Host access rule.

**Result:** The target group reported the instance as Healthy.

## 2. HTTP 504 Gateway Timeout

**Problem:** The webpage initially returned HTTP 504 through the ALB.

**Checks performed:**
- Checked the Nginx service.
- Tested the backend with `curl`.
- Confirmed the backend returned HTTP 200 OK.
- Tested the ALB DNS endpoint from Ubuntu and Windows PowerShell.
- Refreshed the webpage and checked accessibility.

**Result:** The webpage became accessible through the ALB after troubleshooting.

**Note:** The exact cause of the initial 504 was not conclusively established, so no specific configuration change is claimed as its cause.

## 3. Security Group Best Practices

- Allow public HTTP traffic to the ALB on port 80.
- Allow backend HTTP traffic only from the ALB security group.
- Restrict SSH access to the Bastion Host to a trusted source IP.
- Allow private-instance SSH access from the Bastion Host security group only.

## 4. Cost Management

NAT Gateways and Application Load Balancers may incur ongoing charges. Delete resources when the lab is no longer required and verify that no unwanted billable resources remain.