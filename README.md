# AWS 3-Tier Architecture Web Application

A secure and scalable web application deployed on AWS using a 3-Tier Architecture with separate Web, Application, and Database layers.

## Live Application

**[Open Live AWS Application](http://Three-Tier-Public-ALB-1428180970.ap-south-1.elb.amazonaws.com)**

---

## Architecture

**Internet → Public ALB → Web Tier → Internal ALB → Application Tier → Amazon RDS MySQL**

### Web Tier
- Amazon EC2
- Nginx
- HTML frontend
- Public subnets

### Application Tier
- Amazon EC2
- Flask application
- Private subnets
- Port `5000`

### Database Tier
- Amazon RDS for MySQL
- Private database subnets
- Database: `feedbackdb`
- Table: `feedback`

---

## AWS Infrastructure

### Networking
- Custom Amazon VPC
- 2 Availability Zones
- 2 Public/Web subnets
- 2 Private/Application subnets
- 2 Private/Database subnets
- Internet Gateway
- NAT Gateway
- Elastic IP
- Route Tables

### Load Balancing
- Internet-facing Public Application Load Balancer
- Internal Application Load Balancer
- Target Groups
- Health Checks

### Compute
- EC2 Web servers
- EC2 Flask application servers

---

## Application Flow

1. User opens the Public ALB URL.
2. Public ALB forwards traffic to the Web Tier.
3. Nginx serves the frontend.
4. The frontend sends requests to `/api/submit`.
5. Nginx forwards API requests to the Internal ALB.
6. Internal ALB forwards requests to the Flask application.
7. Flask processes the feedback.
8. Flask stores the feedback in Amazon RDS MySQL.
9. The response is returned to the user.

---

## Nginx Reverse Proxy

Nginx is used to:

- Serve the frontend
- Handle HTTP requests
- Forward `/api/` requests
- Connect the Web Tier with the private Application Tier

---

## Database

**Engine:** MySQL

**Database:** `feedbackdb`

**Table:** `feedback`

The feedback table stores:

- ID
- Name
- Email
- Feedback
- Created timestamp

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

## Security Architecture

Security is implemented using a layered approach to isolate the Web, Application, and Database tiers and to minimize unnecessary network exposure.

### Network Isolation

- The Web Tier is deployed in public subnets to receive traffic through the internet-facing Public Application Load Balancer.
- The Application Tier is deployed in private subnets and is accessed through the Internal Application Load Balancer.
- The Database Tier is deployed in private database subnets and is not directly accessible from the public internet.

### Security Groups

Separate Security Groups are used to control traffic between the application tiers.

Traffic is restricted to the required communication paths:

**Internet → Public ALB → Web Tier → Internal ALB → Application Tier → RDS MySQL**

This tier-based access model prevents unnecessary direct communication between external users and private backend resources.

### Load Balancer Security

- The Public ALB provides the entry point for external client requests.
- The Internal ALB is used for private communication between the Web Tier and Application Tier.
- Backend application servers are not exposed directly to the internet.

### Database Security

Amazon RDS MySQL is placed in the private Database Tier.

The database is accessed by the Application Tier rather than directly by internet-facing clients.

### Credential Protection

Database credentials are not stored directly in the GitHub repository.

The application uses the `DB_PASSWORD` environment variable for the database password.

### Security Objective

The security design follows the principle of limiting access to only the resources and communication paths required for the application to operate.
---

## Testing & Validation

The application was validated end-to-end to confirm communication between all three tiers.

### End-to-End Request Flow

**Public ALB → Web Tier → Nginx → Internal ALB → Flask Application → RDS MySQL**

### Validation Performed

- Public ALB successfully served the web application.
- Nginx successfully served the frontend.
- API requests were forwarded through the Internal ALB.
- Flask successfully processed feedback submissions.
- Feedback data was successfully stored in Amazon RDS MySQL.
- Database records were verified in the `feedback` table.

### Application Result

A successful feedback submission returned:

**Feedback submitted successfully!**

The submitted record was then verified in the RDS MySQL database.

This confirms that the Web Tier, Application Tier, and Database Tier are communicating successfully.
---

## Repository Structure

```text
aws-three-tier-architecture/
├── README.md
├── app.py
├── index.html
├── nginx.conf
├── proxy.md
├── App.md
├── DB.md
└── .gitignore
---

## Project Status

| Component | Status |
|---|---|
| Custom VPC | Completed |
| 6 Subnets | Completed |
| Internet Gateway | Completed |
| NAT Gateway | Completed |
| Route Tables | Completed |
| Web Tier | Completed |
| Public Application Load Balancer | Completed |
| Application Tier | Completed |
| Internal Application Load Balancer | Completed |
| Nginx Reverse Proxy | Completed |
| Amazon RDS MySQL | Completed |
| Security Groups | Completed |
| End-to-End Testing | Completed |
| Auto Scaling | Optional Enhancement |

---

## Future Enhancement

EC2 Auto Scaling can be added to the Web Tier to automatically adjust the number of instances based on application demand and improve fault tolerance.

---

## Author

**Lavish Gill**

[GitHub Repository](https://github.com/gilllavish25/aws-three-tier-architecture)
