# AWS-Web-Server-Infrastructure-with-Terraform


This project provisions a secure and scalable AWS infrastructure for a web application using Terraform. It includes a Virtual Private Cloud (VPC), subnets, EC2 instances in an Auto Scaling Group, an Application Load Balancer (ALB), CloudFront CDN, Route 53 DNS records, and associated security configurations.

---

## 🧰 Prerequisites

### Install Required Tools

```bash
# macOS
brew install terraform

# Ubuntu / Debian
sudo apt install -y unzip wget
wget https://releases.hashicorp.com/terraform/1.9.8/terraform_1.9.8_linux_amd64.zip
unzip terraform_1.9.8_linux_amd64.zip
sudo mv terraform /usr/local/bin/
terraform -version
```

### Configure AWS Credentials

Terraform uses your AWS credentials to deploy resources. You can configure them by running:

```bash
aws configure
```

Or by setting environment variables:

```bash
export AWS_ACCESS_KEY_ID=yourAccessKey
export AWS_SECRET_ACCESS_KEY=yourSecretKey
export AWS_DEFAULT_REGION=ap-southeast-1
```

Ensure your AWS IAM user or role has sufficient permissions to create:

* VPC, Subnets, Gateways
* EC2, Auto Scaling, Load Balancer
* IAM Roles, Instance Profiles
* CloudFront, Route53, ACM Certificates

---

## ⚙️ Project Structure

```
terraform-webserver/
├── main.tf               # Main Terraform configuration
├── terraform.tfvars      # Environment variables
├── README.md             # Deployment guide (this file)
```

---

## 📋 Configuration

Create a `terraform.tfvars` file to store your environment-specific variables:

```hcl
aws_region     = "ap-southeast-1"
domain_name    = "yourdomain.com"
hosted_zone_id = "Z123ABCXYZ"
```

Variables used:

* **aws_region** – AWS region to deploy resources.
* **domain_name** – Your registered domain name (must exist in Route53).
* **hosted_zone_id** – The hosted zone ID associated with your domain.

---

## 🚀 Deployment Steps

### 1️⃣ Initialize Terraform

This installs the AWS provider and sets up the backend:

```bash
terraform init
```

### 2️⃣ Validate Configuration

Check syntax and ensure configuration correctness:

```bash
terraform validate
```

### 3️⃣ Review Planned Resources

```bash
terraform plan
```

This shows all AWS resources Terraform will create.

### 4️⃣ Deploy Infrastructure

```bash
terraform apply
```

When prompted, type:

```
yes
```

Deployment takes around 10–15 minutes.

---

## 🌐 Post-Deployment

After Terraform finishes, it outputs:

```
vpc_id = vpc-xxxxxx
alb_dns = webapp-alb-xxxxxx.ap-southeast-1.elb.amazonaws.com
cloudfront_domain = dxxxxx.cloudfront.net
```

Test the deployment:

* Visit the CloudFront domain: `https://dxxxxx.cloudfront.net`
* Visit the ALB domain: `http://webapp-alb-xxxxxx.ap-southeast-1.elb.amazonaws.com`
* After DNS propagation, your domain (e.g. `https://yourdomain.com`) should serve the site.

---

## 🔒 Security Best Practices

| Area    | Recommendation                                            |
| ------- | --------------------------------------------------------- |
| Network | Use private subnets for EC2 and NAT for outbound internet |
| IAM     | Follow least privilege principle                          |
| Access  | Use SSM Session Manager instead of SSH                    |
| HTTPS   | Use ACM certificates (CloudFront + ALB)                   |
| Backups | Enable automated snapshots via AWS Backup                 |
| DDoS    | Enable AWS Shield (standard)                              |
| WAF     | Integrate AWS WAF with CloudFront                         |

---

## 🧹 Cleanup

To delete all created resources and avoid charges:

```bash
terraform destroy
```

Type `yes` to confirm.

---

## 🧱 Optional Add-ons

| Feature    | Add via Terraform                              |
| ---------- | ---------------------------------------------- |
| Database   | RDS (MySQL/PostgreSQL) instance                |
| Caching    | ElastiCache (Redis) cluster                    |
| Monitoring | CloudWatch Alarms, Logs, and Metrics           |
| Backup     | AWS Backup Plan and Vault                      |
| CI/CD      | GitHub Actions or AWS CodePipeline integration |

---

## 🧭 Troubleshooting

| Issue                         | Fix                                                      |
| ----------------------------- | -------------------------------------------------------- |
| ACM Certificate not validated | Ensure DNS validation record exists in Route53           |
| CloudFront not serving HTTPS  | Wait for certificate validation (~15 min)                |
| EC2 not responding            | Check Security Group inbound rules and ALB health checks |

---

## 🏁 Summary

You now have a production-grade AWS infrastructure using Terraform, featuring:

* ✅ Secure VPC with public/private subnets
* ✅ Scalable EC2 in Auto Scaling Group
* ✅ Load balancing with ALB
* ✅ Global caching via CloudFront
* ✅ DNS and SSL with Route53 + ACM

---

**Next steps:** Add RDS, WAF, and CI/CD pipeline for a complete enterprise-ready setup.

