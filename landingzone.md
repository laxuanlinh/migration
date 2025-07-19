# Organization and OU
- A project might have 1 org specialized for a domain, in this case we have 1 ORG
- Multi-account strategy that uses AWS ORG to manage and group
- Different accounts for differnt aspects/environments: Core, security, infra, non-prod, prod, sandbox, archive
- `Core`: log archive, audit. this is created by default by Control Tower
- `Security`: security, guard duty, config recorder
- `Infra`: network, account factory, platform
- `Non-prod`: dev, uat, staging
- `Prod`: prod
- `Sandbox`: testing, prototyping
- `Archive`: suspended accounts
## Control Tower design
- IAM Identity Center integrates with AAD to enable SSO, doesn't have to create new account or user, it maps to AAD user and permission sets
- Control Tower creates new accounts with roles, it allows AAD user to assume those roles.
- Each account created is provisioned with a VPC (3 AZs, subnets), VPC gateway endpoint, private/public domain name (route53), GuardDuty enabled...
- Based on this understanding, I'm guessing:
    - `Core`: 1 account
    - `Security`: 1 account
    - `Infra`: 1 account
    - `Non-prod`: 3 accounts
    - `Prod`: 1 account
    - `Sandbox`: 1 account
    - `Archive`: by default 0
- We're following CIS AWS Foundations v1.4 Level 1 Benchmarks, the baseline level for general projects
- Beside this security standard, there are also:
    - NIST 800-53 (moderate or high baseline)
    - ISO/IEC 27001
    - PCI DSS (if handling cardholder data)
    - HIPAA (for healthcare data in the US)
- We apply Guardrails which are rules to prevent or detect violations, there are Mandatory rules for all OUs, Recommended for Prod and Security, and Optional
# Networking design
- At the center is the `Transit Gateway`, used to connect all VPCs and traffic from outside (on-prem, internet)
## VPC
- Each account has a VPC:
    - Sandbox
    - Non-prod
    - Prod
    - Management (Core, Infra)
    - Inspection (Security)
- Outbound VPC with NAT gateway
- Inbound VPC with ELB and ACM
- Inspection VPC: all traffic from Transit Gateway goes through here for Firewall before going to other VPCs or outside
## From on-prem
- We're suggesting using `Direct Connect` from DC to Transit Gateway for better performance, 2 fibreoptic cables to 2 ISPs for redundancy 
- Per the cost estimation this is quite expensive in both time and money, 10k per year for 2 ports, plus time to work with both ISPs to set up.
- Connection from DR is via site-to-site as it's not as critical
## VPC design
- 3 AZs with 3 subnets, 1 for app, 1 for data and 1 for TGW ENI because it requires its own subnet
- Traffic through TGW will go to the closest ENI and go to the appropriate subnet/service based on the IP address
- For outbound traffic, the go through the route table of its own subnet to decide whether to go within the subnet, VPC or to the TGW.
- The TGW has its own route tbale to route to appropriate VPC
- In theory it's possible that 2 subnets of 2 different VPCs in 2 different AZs can have the same IP range but in practice they should not, so that it's easier to define the route tables
- Traffic to other managed services like RDS, MSK is through TGW and then `Shared service VPC` for centralized VPC endpoint.
- Traffic to S3 and Dynamo which require gateway endpoints would have to be set up per VPC
## Shared service VPC
- For common services that other VPCs use, like VPC endpoints (to managed sevices) and Route 53 resolver endpoint
- S3 and Dynamo doesn't go through here as said, they are region-bound, not VPC-bound

## Inspection VPC
- 3 AZs like other VPCs, each with 2 subnets for firewall and ENI.
- After inspecting traffic, likely forwards it back to the TGW because TGW has the route table to route to proper VPC

## Inbound VPC
- **Is there a WAF attached to ALB???**
- **Looks like we route traffic to Inspection VPC for AWS Firewall L3/L4, but no WAF for L7 at ALB**
- ACM is used to expose https and for ALB to terminate TLS
- 3 AZs, 2 subnets, 1 for ALB, 1 for TGW ENI, **no signs of WAF or ACM**

## Outbound VPC
- Similar to Inbound, all traffic through TGW and Firewall
- 2 subnets, 1 for ENI, 1 for NAT

## TGW design
- Each VPC -> TGW via a TGW Attachment (ENI + metadata), each attachment has its own route table and TGW choose the route table based on which VPC the traffic from
- For Prod and Non-prod, we have 2 different routing domains in the form of 2 different route tables.
- For traffic from on-prem, though it's via Direct Connect or VPN, it also has its own attachment to the TGW thus route table.

# DNS design
# Multi-account architecture design
# Logging and monitoring design
# Security design
# Governance
# Automation design