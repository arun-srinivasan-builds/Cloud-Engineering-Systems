<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Private Service Connectivity (S3) - VPC Endpoints

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-endpoints)

**Author:** Arun Srinivasan  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-tech

---

## Navigation

| Previous | Next |
|----------|------|
| [Network Security Groups](./04-network-security-groups.md) | [S3 Networking](./06-s3-networking.md) |

---
## Execution Metadata

**Execution Type:** Canonical  
**Audience:** Builder | Designer | Learner  
**Production Use:** Yes  
**System Dependency:** Required

**Prerequisites:**
- VPC and private subnets configured
- Understanding of AWS service endpoints
- Knowledge of Gateway vs. Interface endpoints
- Understanding of private connectivity patterns
- Access to S3 and DynamoDB services

**Outputs:**
- VPC Gateway Endpoint configuration (S3, DynamoDB)
- VPC Interface Endpoint configuration (if applicable)
- Private connectivity validation results
- Route table updates for endpoint routing
- Cost and connectivity test evidence

---

## VPC Endpoints

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_09bcaa8a)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon Virtual Private Cloud provides an isolated network boundary within AWS where IP addressing, subnet layout, routing, and traffic controls are explicitly defined. It exists to give operators deterministic control over network behavior, isolation, and service exposure while supporting scalable cloud workloads.

### How I used Amazon VPC in this project

This project uses VPC endpoints to enable private connectivity from compute resources to AWS-managed services. Traffic remains on the AWS backbone and does not traverse the public internet, reducing exposure while simplifying network egress controls.

### One thing I didn't expect in this project was...

The project highlights how AWS-managed service access can be fully private without additional network appliances. Service integration is achieved through native VPC constructs rather than external connectivity patterns.

### This project took me...

The implementation was completed within a short execution window and focused on validating VPC endpoints, private subnets, and controlled S3 access paths rather than optimizing for scale or production readiness.

---

## In the first part of my project...

### Step 1 - Architecture set up

A baseline network was established consisting of a VPC, subnets, routing, and security controls. An EC2 instance was deployed as a workload endpoint, and an S3 bucket was created as a target service. This foundation supports testing private service access patterns without relying on internet gateways.

### Step 2 - Connect to EC2 instance

Access to the EC2 instance was established using AWS-managed connectivity mechanisms. The intent was to validate instance reachability while keeping network traffic scoped to AWS-managed paths.

### Step 3 - Set up access keys

Credentials and permissions were configured to allow the EC2 instance to authenticate to AWS APIs. This step exists to enable controlled service access for validation, while highlighting the security implications of credential management in private environments.

### Step 4 - Interact with S3 bucket

The EC2 instance was configured to access S3 through a VPC endpoint. This validates that object storage operations can occur without public internet routing and confirms that service access aligns with private network design goals.

---

## Architecture set up

The VPC defines IP ranges, subnet segmentation, and routing behavior to control traffic flow.

The S3 bucket provides a managed storage endpoint used to validate private service access patterns rather than serve as a production data store.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_4334d777)

---

## Access keys

### Credentials

VPC endpoints, security groups, route tables, subnet associations, and DNS resolution were aligned to support private AWS service access from EC2. Credential handling is required for API authentication but remains decoupled from network path enforcement.

AWS access keys represent identity credentials managed by IAM. They exist to authenticate requests and must be protected due to their ability to authorize actions across services.

Improper handling of secret access keys introduces risk. Secure storage, rotation, and avoidance of hardcoded credentials are required to prevent unauthorized access.

### Best practice

IAM roles attached to EC2 instances replace long-term credentials with temporary, automatically rotated credentials. This approach reduces exposure and aligns with AWS-recommended access patterns for VPC endpoint usage.

---

## Connecting to my S3 bucket

The aws s3 ls command enumerates accessible S3 buckets and serves as a basic validation of authentication and authorization.

Failure indicates credential or policy misalignment rather than network connectivity issues.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_4334d778)

---

## Connecting to my S3 bucket

Listing objects within a specific bucket validates both service reachability and bucket-level permissions. Errors at this stage indicate misconfigured IAM policies, bucket policies, or endpoint restrictions.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_4334d779)

---

## Uploading objects to S3

Object uploads validate write permissions and confirm that data paths flow through the VPC endpoint rather than the public internet.

Endpoint configuration and subnet routing determine whether traffic remains private.

Subnet inspection confirms the absence of public IP assignment. This ensures the instance cannot initiate direct internet traffic, reinforcing the private access model.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_3e1e79a2)

---

## In the second part of my project...

### Step 5 - Set up a Gateway

An S3 gateway VPC endpoint was created to provide private, scalable access to S3 without NAT or internet gateways. This simplifies the architecture while enforcing controlled service access.

### Step 6 - Bucket policies

Bucket policies were applied to restrict access to requests originating from the VPC endpoint. This enforces network-level trust boundaries in addition to IAM authorization.

### Step 7 - Update route tables

Route tables were updated to direct S3-bound traffic to the gateway endpoint. Correct routing is required for endpoint enforcement and to prevent unintended egress paths.

### Step 8 - Validate endpoint conection

Connectivity testing confirms that access succeeds only through the intended endpoint. This step verifies that network and policy controls are aligned and effective.

---

## Setting up a Gateway

The S3 gateway endpoint provides private connectivity between the VPC and S3. It removes dependency on public routing while relying on AWS-managed infrastructure for scalability and availability.

### What are endpoints?

VPC endpoints enable private connectivity to AWS services by anchoring service access within the VPC. They reduce attack surface, simplify routing, and provide deterministic traffic paths.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_09bcaa8a)

---

## Bucket policies

Bucket policies define resource-level permissions for S3. They operate independently of IAM user policies and are used to enforce access constraints based on source, identity, or service context.

The applied policy restricts bucket access to requests originating from the configured VPC endpoint. This ensures that even valid credentials cannot access the bucket from outside the defined network boundary.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_7316a13d)

---

## Bucket policies

The initial policy restricted access by account alone and did not account for the VPC endpoint context. The policy was updated to allow access via the endpoint identifier, enabling private service traffic while maintaining isolation.

Traffic routing was verified to ensure requests traverse the endpoint path and bypass internet gateways.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_4ec7821f)

---

## Route table updates

Route table changes associate S3 traffic with the gateway endpoint. Validation includes confirming route entries, verifying target associations, and testing service access from private subnets.

Terminal output reflects route creation status, existing route conflicts, or permission constraints. These outputs are used to confirm configuration state rather than assess performance.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_d116818e)

---

## Endpoint policies

Endpoint policies further constrain which resources and actions are permitted through the VPC endpoint. They act as an additional authorization layer beyond IAM and bucket policies.

Policy updates were validated through direct testing from the EC2 instance, confirming that access succeeded only under the defined constraints and failed outside them.

![Image](http://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/aws-networks-endpoints_3e1e79a3)

---

---

