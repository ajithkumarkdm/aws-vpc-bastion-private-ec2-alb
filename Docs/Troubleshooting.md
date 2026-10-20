# AWS VPC Hands-On Project — Troubleshooting Guide

## 1. ALB Target Health: Connection Timeout

### Problem
The Application Load Balancer (ALB) target group reported a connection timeout for the private EC2 instance.

### Investigation
- Checked whether the private EC2 instance was running.
- Verified that the Nginx service was running.
- Tested the backend HTTP response.
- Reviewed the target group health check configuration.
- Checked the security group rules for the ALB and private EC2 instance.

### Resolution
Added an inbound HTTP rule to the private EC2 security group:

- **Type:** HTTP
- **Port:** 80
- **Source:** ALB security group

Retained the required Bastion Host access rule.

### Validation
The target group subsequently reported the private EC2 instance as **Healthy**.

---

## 2. HTTP 504 Gateway Timeout

### Problem
The webpage initially returned an HTTP 504 Gateway Timeout when accessed through the ALB DNS endpoint.

### Investigation
- Checked the Nginx service status on the private EC2 instance.
- Tested the backend using `curl`.
- Confirmed that the backend returned HTTP 200 OK.
- Tested the ALB DNS endpoint from Ubuntu and Windows PowerShell.
- Refreshed the webpage and checked whether it remained accessible.

### Result
The webpage became accessible through the ALB after troubleshooting.

**Note:** The exact cause of the initial 504 error was not conclusively established. Therefore, no particular configuration change is claimed as the confirmed cause.

---

## 3. Security Group Configuration

### Recommended rules

**ALB Security Group**
- Allow inbound HTTP (port 80) from `0.0.0.0/0` for public website access.
- Allow outbound traffic to the backend as required.

**Private EC2 Security Group**
- Allow inbound HTTP (port 80) from the ALB security group.
- Allow inbound SSH (port 22) from the Bastion Host security group.
- Avoid exposing the private web server directly to the internet.

**Bastion Host Security Group**
- Allow inbound SSH (port 22) only from a trusted administrator IP address.
- Permit the required outbound SSH connection to the private EC2 instance.

---

## 4. Private EC2 Internet Connectivity

### Problem
A private EC2 instance may not be able to download packages or receive operating system updates.

### Checks
- Confirm that the private subnet is associated with the correct route table.
- Verify that the private route table has a default route (`0.0.0.0/0`) pointing to the NAT Gateway.
- Check that the NAT Gateway is available.
- Verify that the NAT Gateway is in a public subnet with a route to the Internet Gateway.
- Check the instance's outbound security group rules and network ACLs.

### Expected Result
The private EC2 instance should be able to initiate outbound internet connections for package downloads and updates without needing a public IP address.

---

## 5. Validation Checklist

- [ ] VPC and subnet configuration verified.
- [ ] Public and private route tables configured correctly.
- [ ] NAT Gateway available for private-subnet outbound connectivity.
- [ ] Bastion Host SSH access verified.
- [ ] Nginx running on the private EC2 instance.
- [ ] Backend HTTP response returns HTTP 200 OK.
- [ ] ALB target group reports Healthy.
- [ ] Website accessible through the ALB DNS endpoint.
- [ ] Private EC2 instance is not directly exposed to the internet.

---

## 6. Cost Management

AWS NAT Gateways and Application Load Balancers can incur ongoing charges.

After completing the lab:

- Delete resources that are no longer required.
- Check for remaining EC2 instances, NAT Gateways, load balancers, public IPv4 addresses, and other billable resources.
- Review AWS Billing and Cost Management to check for unexpected charges.

---

## Key Learning

This troubleshooting exercise strengthened practical understanding of AWS VPC routing, Bastion Host access, Security Groups, NAT Gateway connectivity, ALB target health checks, Nginx validation, and HTTP troubleshooting.