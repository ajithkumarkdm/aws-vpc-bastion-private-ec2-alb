# AWS VPC Hands-On Project: Bastion Host, Private EC2 & Application Load Balancer

## Project Overview

This project demonstrates how to build and troubleshoot an AWS network architecture using Amazon VPC, public and private subnets, a Bastion Host, a private EC2 web server, NAT Gateway, and an Application Load Balancer (ALB).

The objective was to deploy an Nginx web server on a private EC2 instance and make the webpage accessible through an internet-facing ALB without exposing the web server directly to the internet.

## Architecture

**Traffic flow:**

Internet → Application Load Balancer → Private EC2 → Nginx

**Administrative access:**

My computer → Bastion Host → Private EC2

## AWS Services Used

- Amazon VPC
- Public and private subnets
- Internet Gateway (IGW)
- NAT Gateway
- Route tables
- Amazon EC2 (Ubuntu)
- Security Groups
- Application Load Balancer (ALB)
- Target Groups and health checks
- Nginx web server

## Network Configuration

| Resource | Configuration |
|---|---|
| VPC | 10.10.0.0/16 |
| Public subnet 1 | 10.10.1.0/24 |
| Private subnet | 10.10.2.0/24 |
| Public subnet 2 | 10.10.3.0/24 |
| Bastion Host | 10.10.1.125 |
| Private web server | 10.10.2.180 |
| Web server | Nginx, HTTP port 80 |
| Load balancer | Internet-facing ALB |

The public subnets were configured in separate Availability Zones. The private web server was deployed in a private subnet.

## Implementation Steps

1. Created a custom VPC and public/private subnets.
2. Attached an Internet Gateway and configured public route tables.
3. Created a NAT Gateway and configured the private route table for outbound internet access.
4. Launched an Ubuntu Bastion Host in a public subnet.
5. Launched a private Ubuntu EC2 instance.
6. Connected to the private instance through the Bastion Host using SSH.
7. Installed and started Nginx on the private EC2 instance.
8. Created an ALB, target group, and HTTP listener on port 80.
9. Configured Security Groups to allow the required traffic.
10. Verified the Nginx webpage through the ALB DNS name.

## Troubleshooting and Solutions

### 1. ALB Target Health: Connection Timeout

**Problem:** The target group reported a connection timeout while checking the private EC2 instance.

**Investigation:** Verified the Nginx service, backend HTTP response, target port, and Security Group rules.

**Resolution:** Added an inbound HTTP rule on port 80 to the private EC2 Security Group with the ALB Security Group as the source, while retaining the required Bastion Host access rule.

**Validation:** Verified that the target became Healthy.

### 2. HTTP 504 Gateway Timeout

**Problem:** The webpage initially returned a 504 Gateway Timeout through the ALB.

**Troubleshooting performed:**

- Checked the Nginx service status.
- Tested the backend using `curl`.
- Confirmed that the private web server returned HTTP 200 OK.
- Tested the ALB DNS endpoint from Ubuntu and Windows PowerShell.
- Refreshed the webpage and verified that it remained accessible.

**Result:** The webpage was accessible through the ALB after troubleshooting.

*Note: The specific cause of the initial 504 should be confirmed from the final diagnostic evidence if it needs to be attributed to a particular configuration change.*

## Validation

- Nginx service running on the private EC2 instance.
- Backend HTTP response verified.
- ALB target health verified as Healthy.
- Webpage accessed through the ALB DNS endpoint.
- Webpage remained accessible after browser refresh.

## Key Learnings

- VPC and subnet design
- Public versus private subnet routing
- Internet Gateway versus NAT Gateway
- Bastion Host access to private instances
- Security Group traffic control
- ALB listeners, target groups, and health checks
- HTTP connectivity testing with `curl`
- Troubleshooting network timeouts and HTTP 504 errors


## Project Screenshots

### 1. VPC Overview
![VPC Overview](screenshots/01%20%E2%80%94%20VPC%20overview.png)

### 2. Subnets and Availability Zones
![Subnets](screenshots/02%20%E2%80%94%20Subnets%20and%20Availability%20Zones.png)

### 3. Route Tables
![Route Tables](screenshots/03%20%E2%80%94%20Route%20tables.png)

### 4. EC2 Instances
![EC2 Instances](screenshots/04%20%E2%80%94%20EC2%20instances.png)

### 5. Target Group Health
![Target Group Health](screenshots/05%20%E2%80%94%20Target%20group%20health.png)

### 6. Application Load Balancer
![Application Load Balancer](screenshots/06%20%E2%80%94%20Application%20Load%20Balancer.png)

### 7. Nginx HTTP 200 Validation
![Nginx HTTP 200](screenshots/07%20%E2%80%94%20Nginx%20and%20HTTP%20200%20test.png)

### 8. Final Webpage
![Final Webpage](screenshots/08%20%E2%80%94%20Final%20webpage.png)


Planned screenshots:

1. VPC overview
2. Public and private subnets
3. Route tables
4. EC2 instances
5. Target group health
6. ALB listener and configuration
7. Nginx HTTP 200 response
8. Final webpage through the ALB
9. Troubleshooting evidence


## Documentation

- [Troubleshooting Guide](docs/troubleshooting.md)


## Author

**Cloud Support | AWS | Linux | Networking | DevOps**

This project was completed as a hands-on learning exercise to strengthen practical skills in AWS networking, EC2 administration, security groups, load balancing, and cloud troubleshooting.

GitHub: https://github.com/ajithkumarkdm
