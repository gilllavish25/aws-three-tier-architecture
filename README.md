# AWS 3-Tier Architecture Web Application

A secure, scalable web application built using AWS 3-Tier Architecture with separate Web, Application, and Database tiers.

## Live Application

**[Open Live AWS Application](http://Three-Tier-Public-ALB-1428180970.ap-south-1.elb.amazonaws.com)**

The application provides a feedback form and processes requests through the complete 3-tier architecture.

---

## Architecture

```text
Internet
    |
    v
Public Application Load Balancer
    |
    v
Web Tier
EC2 + Nginx
Public Subnets
    |
    | /api/
    v
Internal Application Load Balancer
    |
    v
Application Tier
Flask + EC2
Private Subnets
Port 5000
    |
    v
Database Tier
Amazon RDS MySQL
Private Subnets
---

## AWS Infrastructure

The project uses a custom Amazon VPC deployed across two Availability Zones.

### VPC and Networking

- Custom Amazon VPC
- 2 Availability Zones
- 2 Public/Web subnets
- 2 Private/Application subnets
- 2 Private/Database subnets
- Internet Gateway
- NAT Gateway
- Elastic IP
- Public route table
- Private Application route table
- Private Database route table

### Web Tier

Two EC2 instances are used for the Web Tier.

Services:
- Amazon EC2
- Nginx
- HTML frontend

The Web Tier is deployed in the public subnets.

### Public Application Load Balancer

An internet-facing Application Load Balancer distributes incoming user traffic to the Web Tier.

```text
Internet
    |
    v
Public ALB
    |
    +---- Web Server A
    |
    +---- Web Server B
---

## Security

The application is designed with separate security controls for the Web, Application, and Database tiers.

### Security Groups

Traffic between the different tiers is restricted using AWS Security Groups.

The intended traffic flow is:

```text
Internet
    |
    v
Public ALB
    |
    v
Web Tier
    |
    v
Internal ALB
    |
    v
Application Tier
    |
    v
RDS MySQL
---

## Database Tier

Amazon RDS for MySQL is used as the persistent database layer.

### Database Configuration

Database engine:

`MySQL`

Database name:

`feedbackdb`

Table:

`feedback`

The RDS instance is deployed in the private Database subnets.

### Database Schema

```sql
CREATE TABLE feedback (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    feedback TEXT,
    created TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
---

## Testing and Validation

The complete 3-tier application was tested through the public Application Load Balancer.

### End-to-End Flow

```text
Public ALB
    |
    v
Web Tier
    |
    v
Nginx Reverse Proxy
    |
    v
Internal ALB
    |
    v
Flask Application Tier
    |
    v
Amazon RDS MySQL
---

## Repository Structure

```text
aws-three-tier-architecture/
│
├── README.md
├── app.py
├── index.html
├── nginx.conf
├── proxy.md
├── App.md
├── DB.md
└── .gitignore
---

## Future Enhancement

EC2 Auto Scaling can be added to the Web Tier as an optional scalability and fault-tolerance enhancement.

---

## Author

**Lavish Gill**

GitHub Repository:

[aws-three-tier-architecture](https://github.com/gilllavish25/aws-three-tier-architecture)
