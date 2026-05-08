# AWS Playbooks

Ansible playbooks for provisioning and managing AWS infrastructure. Designed to run from Ansible Automation Platform (AAP) Controller with AWS credentials injected via a credential type.

---

## Requirements

```yaml
# collections/requirements.yml
collections:
  - name: amazon.aws
  - name: community.aws
```

AWS credentials are passed via environment variables (injected by AAP Controller):

```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
```

---

## Playbooks

### VPC & Networking

| Playbook | Description |
|---|---|
| `createvpc.yml` | Create a VPC with subnets, internet gateway, and route tables |
| `createsecuritygrp.yml` | Create a security group with defined inbound/outbound rules |
| `deletevpc.yml` | Tear down a VPC and all associated resources |

### EC2 Instances

| Playbook | Description |
|---|---|
| `createvms.yml` | Launch one or more EC2 instances into a target VPC/subnet |
| `deleteinstance.yml` | Terminate an EC2 instance by ID or tag |
| `instancecontrol.yml` | Start, stop, or reboot EC2 instances |
| `getinstanceinfo.yml` | Gather and display EC2 instance facts (state, IP, tags) |
| `getpubipofwebserver.yml` | Retrieve the public IP of a web server instance by tag |

### SSH Keys

| Playbook | Description |
|---|---|
| `addsshkey.yml` | Import an SSH public key to AWS as a named key pair |

---

## Variable Examples

Variables are passed via AAP Controller surveys or extra vars. Each playbook defines its required variables. Typical variables:

```yaml
# VPC
vpc_name: my-demo-vpc
vpc_cidr: 10.0.0.0/16
aws_region: us-east-1

# EC2
instance_type: t2.micro
ami_id: ami-xxxxxxxxxxxxxxxxx
key_name: my-key-pair
instance_name: my-demo-instance
security_group: my-sec-group
subnet_id: subnet-xxxxxxxxxxxxxxxxx
```

---

## AAP Controller Setup

1. Create an **AWS credential** in AAP Controller (type: Amazon Web Services)
2. Create a **Project** pointing to this repository
3. Create **Job Templates** for each playbook, attaching the AWS credential
4. Optionally add **Surveys** for dynamic variable input (instance type, region, etc.)
