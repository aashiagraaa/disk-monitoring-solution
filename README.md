# Multi-Account AWS Disk Monitoring Solution

Scalable disk monitoring solution for AWS multi-account environments using Ansible, Systems Manager (SSM), and CloudWatch.

## Architecture

![Architecture Diagram](docs/architecture_diagram.png)

## Solution Components

### 1. Access Management
- **AWS Systems Manager (SSM)**: Secure, agentless access without SSH keys
- **IAM Cross-Account Roles**: Ansible assumes roles using STS
- **No Open Ports**: All communication over HTTPS

### 2. VM Discovery and Enrollment
- **Tag-Based Discovery**: Instances tagged with `MonitorDisk=true` auto-discovered
- **Dynamic Inventory**: Real-time discovery across accounts and regions
- **Automated Enrollment**: New VMs automatically enrolled when tagged

### 3. Data Collection and Aggregation
- **CloudWatch Agent**: Collects disk metrics (usage %, free space, I/O)
- **Cross-Account Streaming**: All metrics to central monitoring account
- **Lambda Aggregation**: Hourly aggregation and S3 storage for historical analysis

### 4. Scalability
- **Multi-Account Support**: Works across AWS Organization accounts
- **Multi-Region Support**: Discovers VMs in all configured regions
- **Tested Scale**: 5000+ instances
- **Horizontal Scaling**: Add accounts/regions via configuration

## Quick Start

### Prerequisites
```bash
pip install -r requirements.txt
ansible-galaxy collection install -r requirements.yml
```

### Configuration
Edit `inventory/group_vars/all.yml` with your AWS account IDs and settings.

### Deployment
```bash
# Discover VMs
ansible-playbook playbooks/01_discover_vms.yml

# Deploy CloudWatch Agent
ansible-playbook playbooks/02_deploy_cloudwatch_agent.yml

# Setup monitoring and alerts
ansible-playbook playbooks/03_setup_monitoring.yml
```

## Project Structure

```
.
├── README.md                          # This file
├── SOLUTION_SUMMARY.md                # Detailed solution overview
├── docs/
│   ├── architecture_diagram.png       # Architecture diagram
│   └── iam-policies/                  # IAM policy documents
├── playbooks/
│   ├── 01_discover_vms.yml           # VM discovery
│   ├── 02_deploy_cloudwatch_agent.yml # Agent deployment
│   └── 03_setup_monitoring.yml       # Monitoring setup
├── roles/
│   ├── vm_discovery/                 # Discovery role
│   └── cloudwatch_agent/             # Agent deployment role
├── inventory/
│   └── group_vars/all.yml            # Configuration
└── scripts/
    └── lambda_aggregator.py          # Metric aggregation Lambda
```

## Key Features

- ✅ No SSH keys required
- ✅ Secure cross-account access
- ✅ Automatic VM discovery
- ✅ Real-time disk monitoring
- ✅ Multi-threshold alerts
- ✅ Centralized dashboards
- ✅ Scales to 5000+ instances

## Cost
~$1.25 per instance per month

## Security
- Least privilege IAM policies
- No inbound ports required
- Encrypted metrics (TLS + KMS)
- Full audit trail via CloudTrail
# disk-monitoring-solution-optimized
