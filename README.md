# Multi-App Hosting Using AWS Network Load Balancer (NLB)

## Project Overview

This project demonstrates hosting multiple web applications on EC2 instances and distributing traffic using AWS Network Load Balancer.

---

## Architecture

Users → Network Load Balancer → EC2 Instances

---

## Prerequisites

- AWS Account
- Two EC2 Instances
- Security Group allowing HTTP (80) and SSH (22)
- GitHub Repositories containing application code

---

## Step 1: Launch Two EC2 Instances

Create:

- web-server-1
- web-server-2

Availability Zones:

- ap-south-1a
- ap-south-1b

Security Group:

- SSH (22)
- HTTP (80)

---

## Step 2: Install Apache Web Server

Connect to both instances:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

---

## Step 3: Deploy Applications from GitHub

### Server 1 – Google Application

```bash
sudo yum install git -y
cd /var/www/html
sudo rm -rf *
sudo git clone <GitHub-Repository-URL> .
sudo systemctl restart httpd
```

Verify:

```text
http://EC2-1-Public-IP
```

---

### Server 2 – Drive Application

```bash
sudo yum install git -y
cd /var/www/html
sudo rm -rf *
sudo git clone <GitHub-Repository-URL> .
sudo systemctl restart httpd
```

Verify:

```text
http://EC2-2-Public-IP
```

---

## Step 4: Create Target Group

Navigate:

EC2 → Target Groups → Create Target Group

Configuration:

```text
Target Type : Instances
Protocol    : TCP
Port        : 80
Name        : tg-nlb-app
```

Register:

- web-server-1
- web-server-2

---

## Step 5: Configure Health Checks

Settings:

```text
Protocol : TCP
Port     : Traffic Port
```

Verify:

```text
Healthy
Healthy
```

---

## Step 6: Create Network Load Balancer

Navigate:

EC2 → Load Balancers → Create Load Balancer

Select:

```text
Network Load Balancer
```

Configuration:

```text
Name       : multi-app-nlb
Scheme     : Internet-facing
IP Type    : IPv4
```

Select two Availability Zones.

---

## Step 7: Configure Listener

```text
Protocol : TCP
Port     : 80
```

Forward traffic to:

```text
tg-nlb-app
```

Create Load Balancer.

---

## Step 8: Verify Health Status

Navigate:

EC2 → Target Groups → Targets

Expected:

```text
web-server-1 → Healthy
web-server-2 → Healthy
```

---

## Step 9: Test Load Balancer

Copy NLB DNS Name:

```text
http://multi-app-nlb-xxxxxxxx.elb.amazonaws.com
```

Open in browser.

Refresh multiple times.

Expected Output:

```text
Google Application
```

or

```text
Drive Application
```

depending on the target instance selected.

---

## Project Outcome

- Deployed applications from GitHub repositories.
- Configured Apache Web Server.
- Created Target Group and Health Checks.
- Configured Network Load Balancer.
- Achieved High Availability and Load Balancing.

---

## Technologies Used

- AWS EC2
- Network Load Balancer
- Apache HTTP Server
- GitHub
- Linux

---
