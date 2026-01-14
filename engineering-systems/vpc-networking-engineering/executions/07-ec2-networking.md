<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launching VPC Resources - EC2 Networking Behavior

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-ec2)

**Author:** Arun Srinivasan  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-tech

---

## Execution Metadata

**Execution Type:** Reference  
**Audience:** Builder | Designer | Learner  
**Production Use:** Conditional  
**System Dependency:** Optional

**Prerequisites:**
- VPC, subnets, and security groups configured
- EC2 service access
- Understanding of EC2 instance networking
- Knowledge of security group associations
- Basic familiarity with instance launch process
- Understanding of public vs. private IP addresses

**Outputs:**
- EC2 instances in VPC subnets
- Security group associations
- Network interface configurations
- Connectivity validation results
- Instance networking test evidence

---

## Navigation

| Previous | Next |
|----------|------|
| [S3 Networking](./06-s3-networking.md) | [VPC Peering Advanced](./08-vpc-peering-advanced.md) |

---

## Launching VPC Resources

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-ec2_8ee57662)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is an isolated, software-defined network boundary within an AWS account. It defines IP address space, subnet segmentation, routing, and traffic control primitives that govern how compute resources communicate internally and externally.

### How I used Amazon VPC in this project

Amazon VPC was used to establish explicit network boundaries, segment workloads by subnet, and enforce traffic controls through security groups, network ACLs, and route tables. The configuration focused on isolating resources while enabling controlled ingress and egress paths required for EC2 connectivity.

### One thing I didn't expect in this project was...

Security group configuration required more precision than initially assumed. Inbound and outbound rules needed careful scoping to balance accessibility with exposure risk, reinforcing that VPC security is primarily enforced through deliberate rule design rather than default behavior.

### This project took me...

The implementation was completed within approximately one hour. The scope aligned with intermediate VPC and EC2 concepts, including subnet placement, routing configuration, and access control validation.

---

## Setting Up Direct VM Access

Direct VM access enables administrative interaction with the guest operating system for configuration, software installation, diagnostics, and operational control. This access model assumes responsibility for OS-level security and lifecycle management.

### SSH is a key method for directly accessing a VM

SSH provides encrypted, authenticated remote access to Linux-based instances. It supports secure command execution, file transfer, and port forwarding over TCP port 22, using either key-based or password-based authentication.

### To enable direct access, I set up key pairs

EC2 key pairs were used to enable cryptographic authentication without shared credentials. The public key is associated with the instance at launch, while the private key is retained locally and required to establish SSH sessions.

A PEM-formatted private key was used to support OpenSSH-compatible clients. The file encodes the private key material in base64 and must be protected, as possession of the key grants access to the associated instance.

---

## Launching a public server

The EC2 instance was made publicly reachable by placing it in a public subnet, associating appropriate security group rules, and assigning an Elastic IP address to provide a stable external endpoint.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-ec2_88727bef)

---

## Launching a private server

A dedicated security group was created to restrict network access to only required sources and ports. This enforced least-privilege connectivity and reduced the attack surface of the private instance.

My private serverΓÇÖs security groupΓÇÖs source is my personal computerΓÇÖs IP address
Inbound rules were scoped to a single client IP address to ensure that only an explicitly authorized endpoint could initiate connections to the private instance.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-ec2_4a9e8014)

---

## Speeding up VPC creation

AWS CloudFormation was used to define VPC resources declaratively, including subnets, route tables, and security groups. This enabled repeatable provisioning and reduced manual configuration drift.

VPC resource maps visually represent VPC components and their relationships.
The VPC resource map was used to visualize subnet placement, routing paths, and security associations. This view supports validation of network design assumptions and simplifies troubleshooting.

My new VPC uses 10.0.0.0/16. The VPC CIDR block was defined as 10.0.0.0/16. CIDR overlap is permissible across separate VPCs due to isolation, though it introduces constraints for future peering or connectivity scenarios.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-ec2_1cbb1b88)

---

## Speeding up VPC creation

### Tips for using the VPC resource map

Two public subnets, one per Availability Zone (AZ), were chosen Public subnets were distributed across multiple Availability Zones to reduce single-AZ failure impact and support basic availability requirements for internet-facing resources.

NAT gateways were used to allow outbound internet access from private subnets without exposing instances to inbound connections. This design centralizes egress traffic while maintaining subnet isolation, with availability dependent on gateway placement.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-ec2_8ee57662)

---

---
