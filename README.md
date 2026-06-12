# AWS Cloud Platform — Multi-Tier, Highly Available Web Infrastructure

A production-style, multi-tier web application platform on AWS, defined entirely as code with **Terraform**. The project provisions a complete network, deploys a load-balanced application across two Availability Zones, and follows real-world security and high-availability best practices.

Built to demonstrate hands-on skills in **cloud networking, infrastructure-as-code, and AWS architecture**.

---

## Architecture
Internet
                         |
              [ Application Load Balancer ]      <- public subnets (2 AZs)
                   /                  \
        [ App Server A ]        [ App Server B ]  <- private subnets (2 AZs)
          (us-east-1a)            (us-east-1b)
                   \                  /
                [ NAT Gateway -> Internet (outbound only) ]
                **Network design:**
- A custom **VPC** (`10.0.0.0/16`) with DNS support enabled
- **Two public subnets** and **two private subnets**, spread across two Availability Zones for high availability
- An **Internet Gateway** for inbound public traffic
- A **NAT Gateway** so private resources can reach the internet for updates without being publicly exposed
- Separate **public and private route tables** with appropriate associations

**Application tier:**
- An **Application Load Balancer (ALB)** in the public subnets, distributing traffic across servers
- **Two EC2 application servers** in the private subnets, each running a web server
- A **target group** with health checks and an HTTP **listener** wiring it all together

**Security (least privilege):**
- The **ALB security group** accepts HTTP only from the internet
- The **app security group** accepts traffic **only from the ALB**, not the public internet — so application servers are never directly reachable from outside

---

## Skills Demonstrated

| Area | What this project shows |
|------|------------------------|
| **Networking** | VPC design, public/private subnetting across AZs, routing, gateways, security groups — cloud applications of core networking concepts |
| **Cloud (AWS)** | VPC, EC2, Application Load Balancer, NAT Gateway, Internet Gateway, multi-AZ high availability |
| **Infrastructure as Code** | The entire environment is defined in Terraform — reproducible, version-controlled, and destroyable with a single command |
| **Security** | Least-privilege security groups, private application tier, no hard-coded public exposure |

---

## How to Deploy

**Prerequisites:** an AWS account, the AWS CLI configured with credentials, and Terraform installed.

```bash
# 1. Clone the repo
git clone https://github.com/arjunpuri15/Aws-Cloud-platform.git
cd Aws-Cloud-platform

# 2. Initialize Terraform (downloads the AWS provider)
terraform init

# 3. Review what will be created
terraform plan

# 4. Build the infrastructure
terraform apply    # type 'yes' to confirm
```

When the apply completes, Terraform outputs the load balancer URL: alb_url = "http://capstone-alb-xxxxxxxx.us-east-1.elb.amazonaws.com"
Open that URL in a browser. You'll see a response from one of the app servers; refreshing distributes requests across both, demonstrating the load balancer and multi-AZ setup in action.

**To tear everything down:**
```bash
terraform destroy    # type 'yes' to confirm
```

> **Note:** The NAT Gateway, EC2 instances, and load balancer incur small hourly charges. Run `terraform destroy` when you're done to avoid ongoing costs — the infrastructure rebuilds in minutes with `terraform apply`.

---

## Tech Stack

- **Cloud:** AWS (VPC, EC2, ALB, NAT Gateway, Internet Gateway)
- **Infrastructure as Code:** Terraform
- **Region:** us-east-1

---

## Roadmap

Planned enhancements as the project grows:

- [ ] **RDS database** in the private subnets (multi-AZ)
- [ ] Refactor Terraform into reusable **modules**
- [ ] **CI/CD pipeline** (GitHub Actions) for automated plan/apply
- [ ] **Monitoring & alerting** with CloudWatch dashboards

---

## About

Built by **Arjun Puri** as a hands-on cloud engineering project — designing, deploying, and securing real AWS infrastructure entirely through code.

- GitHub: arjunpuri15
- LinkedIn: linkedin.com/in/arjun-puri