# AWS 3-Tier Architecture Web Application

## Project Overview

This project implements a scalable, secure, and high-performance web application using AWS 3-Tier Architecture.

## Architecture

Internet
↓
Public Application Load Balancer
↓
Web Tier (Nginx + HTML)
↓
Internal Application Load Balancer
↓
Application Tier (Flask)
↓
Amazon RDS MySQL
↓
Feedback Database

## AWS Components

- Amazon VPC
- Internet Gateway
- NAT Gateway
- Public and Private Subnets across two Availability Zones
- EC2 Web Tier
- Nginx
- Public Application Load Balancer
- EC2 Flask Application Tier
- Internal Application Load Balancer
- Amazon RDS MySQL
- Security Groups
- Route Tables

## Application Flow

1. User accesses the application through the Public ALB.
2. Public ALB forwards traffic to the Web Tier.
3. Nginx serves the frontend.
4. Requests to /api/ are forwarded to the Internal ALB.
5. Internal ALB forwards requests to the Flask application servers.
6. Flask stores submitted feedback in Amazon RDS MySQL.

## Repository Files

| File | Purpose |
|---|---|
| pp.py | Flask backend application |
| index.html | Feedback form frontend |
| 
ginx.conf | Nginx configuration |
| proxy.md | Nginx reverse-proxy configuration |
| App.md | Application-tier documentation |
| DB.md | Database setup and configuration |
| .gitignore | Prevents secrets and private files from being committed |

## Database

Database:

eedbackdb

Table:

eedback

The application stores submitted name, email, feedback, and creation timestamp in Amazon RDS MySQL.

## Security

Security groups are configured to allow traffic only between the appropriate tiers.

Database credentials are not stored in this repository. The Flask application reads the database password using the DB_PASSWORD environment variable.

## Testing

The complete application was tested through the public load balancer:

Public ALB → Web Tier → Internal ALB → Flask App → RDS MySQL

A feedback submission was successfully stored in the RDS database.

## Future Enhancement

EC2 Auto Scaling can be added to the Web Tier for increased scalability and fault tolerance.

## Author

Lavish Gill
## Live Application

[Open Live AWS Application](http://Three-Tier-Public-ALB-1428180970.ap-south-1.elb.amazonaws.com)

