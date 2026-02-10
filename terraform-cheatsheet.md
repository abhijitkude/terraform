1. Order of terraform provisioning:Every real infrastructure — AWS, Azure, GCP — follows this order:
=========================================================================
Identity
→ Network
→ Entry point
→ Compute
→ Security
→ Observability
→ Automation
========================================================================

2. TERRAFORM PROJECT STRUCTURE (ALWAYS SAME)
======================================================================
project/                        
├── modules/                    modules → reusable building blocks
├── environments/              environments → isolation (dev/stage/prod)
└── README.md                  communication
======================================================================


3. TERRAFORM FILE ROLES (VERY IMPORTANT)
=======================================================================
| File                  | Purpose                           |
| --------------------- | --------------------------------- |
| `main.tf`             | Creates resources / calls modules |
| `variables.tf`        | Defines inputs                    |
| `terraform.tfvars`    | Supplies environment values       |
| `outputs.tf`          | Exposes values to others          |
| `backend.tf`          | Defines state storage             |
| `.terraform.lock.hcl` | Locks provider versions           |
======================================================================

4. VARIABLE PATTERN (ALWAYS SAME)
Pattern
=========================================================================
Variables define what can change ,tfvars define actual values ,main.tf consumes them

variable → value → resource
=========================================================================

Never hardcode values in resources.

5. MODULE PATTERN (CORE CONCEPT)
Every good Terraform module has:
============================================================================
inputs (variables.tf)
→ resources (main.tf)
→ outputs (outputs.tf)
=============================================================================

6. STATE MANAGEMENT PATTERN (CRITICAL)
Production rule
==============================================================================
❌ Never local state

❌ Never commit state

✅ Remote backend

✅ State locking

Standard choice

S3 → state

DynamoDB → lock

Interview-safe line

Remote state enables collaboration and prevents concurrent modifications.
==================================================================================

7. ENVIRONMENT PATTERN (DEV / STAGE / PROD)
==============================================================================
Design principle

Same modules

Different inputs

Different state

environments/
├── dev/
├── stage/
└── prod/
=============================================================================

8. NETWORKING PATTERN (CLOUD-AGNOSTIC)
==============================================================================
Concept	AWS	Azure
Network	VPC	VNet
Public subnet	Public subnet	Subnet + IGW
Firewall	Security Group	NSG
Load balancer	ALB	App Gateway
============================================================================

9. COMPUTE PATTERN (ALWAYS SAME)
=============================================================================
Load Balancer
→ Auto Scaling
→ Compute


Never:

expose EC2 directly

run prod workloads on single instance
============================================================================

10. IAM / ACCESS PATTERN (NON-NEGOTIABLE)
=============================================================================
Golden rules

❌ No access keys on servers

❌ No hardcoding secrets

✅ IAM roles

✅ Least privilege

Interview sentence

Terraform needs credentials to create infra; workloads use IAM roles at runtime.

That line alone sounds senior.
===============================================================================

11. SECRETS PATTERN (UNIVERSAL)
===============================================================================
Platform	Tool
AWS	Secrets Manager / SSM
Azure	Key Vault
Multi-cloud	HashiCorp Vault

Never store secrets in:

tfvars

GitHub

code

Interviewers listen for blast radius awareness.
================================================================================
