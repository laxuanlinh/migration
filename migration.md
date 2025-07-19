# Why migrate?
Why do you want to migrate:
- Accelarate business transformation: business changes all the time, on prem systems just can't cope with it. What if tomrrow business requires a new authentication system or new machine learning capabilities?
- Better agility
- Reduce operating costs: why pay for electricity costs of huge data center, why not focus on business?
- Scalability, performance: managed services, scalable
- Security and compliance: regulation changes, why not go to a provider who already has the blueprints for compliance and security

You need to know why you want to migrate, so that when the migration is complete, you know if you get it, you're able to make better decisions

# Plan, prepare, perform
You have to have a plan, just can't #YOLO it 
Prepare all the things we need for the migration
After everything is prepared, you perform the migration
## Planning: Are you ready?
Need to know if the org is ready for the migration, what are the gaps (technical, business, tools ...?)
**Not sure what it means though, what do tecnical and business gaps look like?**
**What is the framework to do this?**

## Mobilize the resources
Get ready to migrate, fix the gaps. 
Focus on building a baseline environment and developing cloud skills for employees
**What is the framework to fix gaps and build environment and develop skills?**

### Cost Saving - expectation vs reality
Huge misunderstanding in migration by orgs
- During migration process, the cost might peak to pay for:
    - Migration development
    - New tooling and training
    - Duplicate environments during testing
- After the peak, gradually the costs will come down to the target costs

## AWS portfolio
- AWS provides methodology, training, tools and services to migrate from on prem to cloud
- As we need to go through several steps to migrate, there are AWS tools and partners that can help with
- For building inventory: **What do we have?**
    - AWS services: `AWS System manager`, `CloudWatch`, running agents for 2-4 weeks to collect inventory and workload and compile a list of resources that be used in AWS
    - Besides AWS tools, we also have partners like BMC, Deloite, Corent, Flexera, modernizeIT
    - Use *AWS Migration Evaluator* to collect data about the running systems, these are agents collecting metrics like CPU, usage patterns and software inventory, then it's analyzed by the consulting firms and they compile a list of recommended AWS resources and costs saved **Does GFT have this capability? If not, what are we using to evaluate this?**
    - **Based on the landing zone planning, this seems to be lacking in current project, wonder how they're assessing the necessary resources**
- For building business case:
    - To assess if the org is ready to migrate, we have tests:
        - CART: Cloud adoption readiness test
        - MRA: Migration readiness assessment (for AWS partner only)
        - Migration portfolio assessment (AWS partner only)
        - **what do these test?**
    - CloudChomp, Cloudarnize, Cloud Health, turbonomic
- For deep discovery: 
    - BMC, dynatrace, Deloite, Datalog, Corent, Netscout, flexcera, Appdynamics, modernizeIT
- For planning: **We need to plan for the migration**
    - BMC, Deloite, Device42, corent, flexera, modernizeIT
- For landing zone: **The environment that applications and data will reside after migration**
    - `AWS Control Tower` and `AWS Service Catalog`
    - Contino, Bespin Global
- Migrate workload **Migrate all the services and business logic to cloud**
    - `VMWare cloud on AWS` (if using VMWare on prem) or `AWS Application Migration Service`
    - Deloite, attunity, Wandisco, River Meawdow 
- Migrate data **Migrate al lthe data (products, customer ...)**
    - `AWS Storage gateway`, `AWS Transfer Family`, `Database Migration Service DMS`, `DataSync`, `Snow Family`
    - Net App, Cloud Health
- And we also have solutions on AWS Marketplace
- Migrate technologies is easier than migrate people
- We don't actually have to strictly follow this pipeline with these tools, it's up to us to choose which phase to go through and which tools to choose.

# Assess phase
- We need to have an IT snapshot, then we can have answer about why we need to migrate to AWS
**Currently we don't have an IT snapshot**
- After having the snapshot, we need to create a `business case` and bring it to the decision makers so they can decide, usually they don't really know why we need to move to AWS 
- After they have decided to migrate, need to understand the `Inventory`, `what to migrate` and `how ready` we are to migrate
- We also need `tracking` to see where we are in the migration journey
**Our client had clear business cases and decided to go with the migration**
**But are they ready?**
**What are we using to track the journey? Timelines, plans?**
- To build the business case, we can use Migration Evaluator (ME) and Migration Portfolio Assessment (MPA)
- To assess the Inventory, we can use ME and Application Discovery Service (ADS)
- For Readiness, we have Migration Readiness Assessment (MRA) and Cloud Adoptation Readiness tool (CART)
- Finally, to track all of these, we have AWS Migration Hub (MH)
**Do we need these tools?**
## CART and MRA
- CART is public but MRA is for partners
- CART is a self-service online assessment
- MRA is a 1-day workshop with partners or AWS to decide whether we can migrate to AWS
- Both are a set of questions either self-service or asked by AWS
- CART has 47 questions, covering business, people, governace, platform, security and operations
- MRA has 80+ questions
**Seems like if both customers and GFT already have a good idea of what AWS is and if it's ready to migrate, we might not need these tools**
- CART gives score summaries, radar charts, heat maps, reports
- MRA on top of what CART outputs, also provides a PowerPoint presentation about the readiness
**Interestingly, AWS now also offers generative AI chatbot to do the assessment**
*https://cloudreadiness.amazonaws.com/#/*

## Migration Evaluator (formerly TSO Logic)
- Dashboard in AWS
- It helps analyze on-prem compute, storage, memory, microsoft licences...
- Download collector and install on a separate server and let it collect metrics from the actual servers, it's agentless, not actually installed on the running servers
- The data could be in a data storage like S3, we can then feed it into the ME Portal, automatically or manually via flat files
- The output is an `insight report`
- This report can be used by SA or AWS team to feed into MPA

## Application Discovery Service
- Assess phase and Mobilize phase
- Collects server and DB `configuration information`
- Records inbound and outbound traffic, can be used to create a dependency map across all servers
- `Agent based` collector: takes time to install on each machine but can collect `network traffic` and `running processes`
- If using VMware vSphere, can use `Agentless based`, install in vCenter, or can be hybrid, some critical servers can run agents.
- Data from agents -> AWS Application Discovery Service -> AWS Migration Hub
      Agents
        ↓
Discovery Service → S3 → Athena
        ↓                  ↑ 
    Migration Hub       ← Users

### ADS vs ME
- ADS is self service, ME is for partners or AWS team only
- ADS offer agent per server or agentless per vCenter, ME is per Data center
**seems like ME is more hassle free** 
- ADS doesn't have offline data storage, it has to store in `migration hub`, ME can store data in local
**problems with compliance**
- ADS supports variaty of DB like Oracle, SQL Server, Postgres... ME only supports SQL Server
**weird quirk**
- ADS is available in all regions, ME is only available in US East 1

## Migration Porfolio Assessment (MPA)
- Web based, analyze on prem portfolio
- Partner only
- Total Cost of Ownership (TCO) calculation, how much money would cost
- Feature:
    - Data normalization: normalize data collected from on prem
    - Scenario comparison: compare scenarios, what'd happen if I did this, diff plans, diff regions ...
    - AWS cloud economics-based assumption: TCO projection
## How do they all fit?
- ME exports data in a format that can be imported to Migration Hub and MPA as well
- ADS, if ME is not usable for some reason, can be used to feed data to Migration Hub or MPA

## AWS Systems Manager (SSM)
- SSM agents run in on prem and send data back to SSM service and compiled into a report
## CloudWatch
- Monitoring service
- CloudWatch agents running on prem can report back to CloudWatch

# Mobilize phase
## Landing zone
- A zone to land workload
- Building a landing zone requires technical and business decicions about account structure, networking, access...
- Can be built manually or automated with AWS Control Tower, which takes 60-90 mins with wizard
**Can Control Tower be trusted? How much do partners involve?**
## AWS Control Tower
- To setup landing zones: creates accounts, applies guardrails, VPC.
- Doesn't deploy application specific resources
- Control Tower can be used with both Green field and Brown field deployment
- Say we have company 1 with its own AWS Organization with structure of accounts, this company is accquired by company 2 who has its own AWS Org, the solution here is to use Control Tower to setup a landing zone where both of these orgs can be redeployed with centralized policies.
- Provides accounts factory, dashboard to manage compliance.
- Guardrails are restrictions on resources that can be set like public access to S3 bucket, encryption on EBS
- Account Factory is a service that provides accounts based on best practices already setup
- Dashboard shows account compliance, OUs...
## Service Catalog
- Provides templates for deployment
- Infra team can select a template with some customization and Cloud Formation will deploy it in the background
**How does it fit into infra workflow?**
-> Infra team setup an AWS Org with diff OUs (dev, UAT, prod), each can promote infra templates to another with custom params
**How to centralize the whole environment and deploy a new environment?**
-> Infra team defines CloudFormation templates and upload to Service Catalog, this serves as a "artifactory" but for infra templates, they are versioned
**How to customize each environment differently while still centralizing control?**
-> Either share the templates or use pipeline to export from a OU to another, use paramters to customize the specs (instance count, RAM, CPU, DB tier...)
**Seems like GFT is not doing this, we have our own CF templates but keep it somewhere else, don't think we're using Org and OUs to manage accounts and environments**
**We're using diff accs for diff envs and allows devs to access using diff IAM users, harder to manage, separate bills, hard to enforce centralized policies**

# Migration & Modernization phase
## AWS Prescriptive Guidance
- Provide strategies, guides and patterns
- It's a collection of documents/posts

## Migration strategies (7 Rs of migration)
### Relocate
- Say you have VMware running on prem, we can use VMW on AWS to migrate
- Or you have docker containers running, you can move those containers to EKS or ECS
### Rehost (lift-and-shift)
- If you have custom applications that cannot be provided by anyone else, you can lift-and-shift the whole thing to cloud
### Replatform (lift-and-shift)
- If you have old SQL Server, now you can migrate to RDS Aurora
### Refactor
- You need to rearchitect your applications to fit in modern cloud environments
### Repurchase
- Instead of self-managed systems, now you can purchase cloud solutions to do the same things
### Retire
- Some systems are not required because we have cloud alternatives, like LB is replaced by AWS ALB, DNS server by Route53...
### Retain
- Due to compliance, you have to keep some data on-prem so those DB/apps are retained

## Relocate
**Focus on VMWare, Docker/EKS can find other resources**
- VMWare Hypervisor running on top of real hardware in data center converts the physical hardware to virtual hardware which allows multiple virtual machines to run on top of it
- To manage all these VMs, we have vCenter
- For storage, we have VSAN
- For network, we have NSX
- VMC on AWS is AWS provides hardware and VMWare provides software
- Say you have VMWare with vCenter on prem, we can setup VMC that migrates on prem to cloud automatically, not only that, we can also take advantage of other AWS services.
**Is the client running on VMWare Hypervisor on prem? Are there environment specific apps that cannot be run on EKS?**
**If yes, do these apps need to be moved or just the backend services?**
## Rehost
- Lift-and-shift apps from on-prem to AWS without significant changes
- Doesn't take advantage of native services
- AWS has Application Migration Service that can help with lift-and-shift
**maybe can use it if the client ever wants to move to AWS**
- Steps to migrate using MGN:
    - MGN creates a staging area, 
    - We then create a S3 bucket and install Replication agent on the on-prem servers.
    - The agents report to MGN ready to migrate
    - All disks on-prem are replicated as EBS instances but they can only be accessed via EC2
    - MGN creates EC2 instances inside the staging area and connect them to the EBS
    - After all resources are set up, the data starts to flow from the on-prem to AWS, it's a 1-1 mapping
- To test:
    - MGN creats a Production Area
    - EBS are snapshot
    - `Conversion servers` (EC2) are launched while data is still synced from on-prem
    - We use these `Conversion servers` to connect to snapshots to make modifications (lincensing...)
    - These EBS snapshots are deployed to Production Area
    - Use another EC2 instance in Production Area to connect to these EBS instances and test
    - Once we're happy with the data and apps, we can have a cutover and cut off the on-prem and make the production area our main 
    **how does the cutover work, are the snapshots updated?**
- If we want to change some parameters when replicating, we can use `Replication Template` within the MGN
- To launch instances in Production Area, we can use a Launch Template to make changes to the instances
- After the instances been launched, can can make changes to it post launch using `Post Launch Template`
**no idea what these templates are**

## Replatform
- Modifies the existing on-prem apps to work on the cloud (MySQL -> Aurora)
- Native database migration tools for homogenous migration, use vendor tools to create backups and migrate to cloud or use HA tools like Data Guard or CDC
- However if we want to use RDS, we can use DMS to migrate on-prem database to AWS


## Refactor
- Usually when we want to break down monolithic apps into micro services for better availability, scalability ...
- `AWS Migration Hub Refactor Spaces` sets up an infra for refactoring, rewriting apps
- Set up API Gateway -> VPC with subnet -> Transit Gateway that connects to monolithic instances, after refactoring and deploy to another VPC with Fargate, EKS ..., Transit Gateway will reroute to the new VPC

## Repurchase
- Replace legacy systems and move to comsumption-based, SaaS platforms, drop and shop
- AWS Marketplace

## Retire
- Turn off services that are redundant and no longer needed
- Follow some guidelines to make sure that no data loss
- Capture dependencies, inform stakeholders
- Schedule a controlled stop and see what breaks

## Retain
- Retain certain applications on prem while migrating others to cloud due to security, compliance, dependencies, ...

# Data migration
## DataSync
- Bring data from a source to a destination
- Works with file systems, other clouds...
- 3 components:
    - Agent: read and write from sources
    - Locations: source or target endpoints 
    - Tasks: configuration for data transfer and sync
- We set up a connectivity like VPN Connection, Direct connection or internet to connect on-prem with AWS
- Then we install agents on-prem to start reading data and write it to DataSync with controlled bandwith
## Storage Gateway
- Bi-directional service
- Allows applications on-prem to access data in AWS and via versa
- Connects via VPN, Direct Connect or internet
- Can use cache on prem to improve performance.
- Can do backup and restore
- File Gateway: apps talking to a file gateway and it will connects to AWS via HTTP, this is transparent to the apps. Any apps using NFS and SMB can use this type of gateway
- Tape Gateway: Virtual tape presented to on-prem backup apps. It looks like a tape lib but not a tape lib
- Volume Gateway: provides block storage via iSCSI, appears as disk drives, backed by S3 and optionally backed up to EBS
## AWS Transfer Family
- Can send/receive files to/from users or partners over the internet with different protocols
- Supports SFTP, FTPS, FTP
- Can replace on-prem FTP servers
- Can authenticate
- Custom workflows between S3 and EFS using Lambda like encryption
- Most of the time, files pushed to AWS via SFTP
- There are instances when we need to pull/push files from a remote SFTP server
## AWS Snow Family
- 3 online transfer services: Storage gateway, DataSync, Transfer Family
- Another option is an offline solution, copying files to a Snow device and ship it to AWS where the data is offloaded to cloud
- There are 2 types of device in size, Snowcone and Snowball Edge
# Automating Large scale migration
## Cloud Migration Factory
- A solution, not a service, to orchestrate rehosting applications to cloud, powered by Application Migration Service.
- Collection of services, deployed via CloudFormation templates
## Migration Hub Orchestrator
- 