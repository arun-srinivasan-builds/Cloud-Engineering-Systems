<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Access S3 from a VPC

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-s3)

**Author:** Roy Piring Jr  
**Email:** rpiringhawaii@gmail.com

---

## Execution Metadata

**Execution Type:** Reference  
**Audience:** Builder | Designer | Learner  
**Production Use:** Conditional  
**System Dependency:** Optional

**Prerequisites:**
- VPC and private subnets configured
- VPC Gateway Endpoint access
- S3 bucket for testing
- Understanding of S3 access patterns
- Knowledge of bucket policies and IAM policies
- Basic understanding of VPC endpoint routing

**Outputs:**
- S3 VPC Gateway Endpoint configuration
- S3 access validation from private subnet
- Bucket policy configurations
- Private connectivity test results
- Network path validation evidence

---

## Access S3 from a VPC

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-s3_3e1e79a2)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC provides an isolated virtual network boundary for AWS resources. It defines IP addressing, routing, and traffic controls to constrain how resources communicate internally and externally. The VPC establishes the security and network perimeter required for controlled access to managed services such as S3.

### How I used Amazon VPC in this project

The VPC serves as the primary network boundary for an EC2 workload that requires access to Amazon S3. Network configuration limits exposure while enabling private, service-level connectivity to S3, avoiding public internet dependency.

### One thing I didn't expect in this project was...

Configuring a VPC endpoint for S3 requires explicit alignment between subnets, route tables, and security controls. Endpoint placement and associated policies materially affect whether traffic remains private and functional.

### This project took me...

The implementation was completed within a constrained time window and focused on validating core networking concepts: VPC isolation, EC2 execution context, IAM-based access, and private S3 connectivity.

---

## In the first part of my project...

### Step 1 - Architecture set up

A dedicated VPC was created to host an EC2 instance that serves as the execution environment for S3 interactions. This establishes network isolation and deterministic traffic flow.

### Step 2 - Connect to my EC2 instance

Direct access to the EC2 instance enables validation of network reachability, identity configuration, and service access from within the VPC context.

### Step 3 - Set up access keys

AWS access for the EC2 instance is provided through an IAM role with scoped permissions. This avoids static credentials and enforces least-privilege access to S3.

---

## Architecture set up

The VPC functions as the network foundation for the workload, providing controlled routing and security boundaries for the EC2 instance accessing S3.

An S3 bucket was created to act as the target storage service for object operations initiated from within the VPC.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-s3_4334d777)

---

## Running CLI commands

The AWS CLI provides a direct interface for interacting with AWS services from the EC2 instance. Access is governed by the attached IAM role and network configuration.

aws s3 ls verifies visibility of S3 resources from within the VPC execution environment.

aws s3 cp s3://your-bucket-name/file.txt /tmp/file.txt retrieves an object from S3 to the instance filesystem, validating read access and end-to-end connectivity.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-s3_e7fa8776)

---

## Access keys

### Credentials

EC2 access to AWS services is enabled through coordinated configuration of security groups, network interfaces, route tables, IAM roles, DNS resolution, and an S3 VPC endpoint. Together, these components allow private service access without exposing credentials.

Access keys represent static credentials for programmatic AWS access. They enable authenticated API calls but introduce risk if mishandled.

Secret access keys function as confidential authentication material. Exposure results in unauthorized access and potential cost or data impact.

### Best practice

IAM roles attached to EC2 instances replace static access keys and eliminate the need to manage long-lived credentials.

---

## In the second part of my project...

### Step 4 - Set up an S3 bucket

The S3 bucket provides centralized object storage accessible from within the VPC under controlled permissions.

### Step 5 - Connecting to my S3 bucket

The EC2 instance is configured to interact with S3 using its IAM role and private network path.

---

## Connecting to my S3 bucket

aws s3 ls verifies visibility of S3 resources from within the VPC execution environment.

The executed command and resulting output confirm that the EC2 instance can resolve, authenticate, and communicate with S3 using the configured network and identity controls.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-s3_4334d778)

---

## Connecting to my S3 bucket

The successful aws s3 ls output indicates that the AWS CLI is correctly configured and that S3 access is permitted. This validates IAM permissions and VPC-level connectivity to S3.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-s3_4334d779)

---

## Uploading objects to S3

aws s3 cp <local_file_path> s3://<your_bucket_name>/<desired_file_path> uploads a local file to S3, creating a new object within the bucket.

aws s3 cp s3://your-bucket-name/your-object.txt /tmp/downloaded_file.txt retrieves an object from S3, confirming bidirectional access.

aws s3 ls s3://your-bucket-name lists bucket contents from within the VPC, validating that routing, endpoint configuration, and security rules permit outbound S3 access.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-s3_3e1e79a2)

---

---

## Navigation

| Previous | Next |
|----------|------|
| [VPC Endpoints](./05-vpc-endpoints.md) | [EC2 Networking](./07-ec2-networking.md) |
