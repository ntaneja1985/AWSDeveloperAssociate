# AWS Developer Associate Training

## AWS Cloud History
- Launched in 2002
- Launched SQS first and then later on launched SQS, S3 and EC2
- Netflix, Airbnb runs on AWS
- AWS is highest with 31% of market share and Microsoft is second with 25%.
- ![alt text](image.png)
- ![alt text](image-1.png)
- All regions are interconnected.
- Each region has availability zones.
- ![alt text](image-2.png)

### How to choose AWS Region
- ![alt text](image-3.png)
- ![alt text](image-4.png)
- ![alt text](image-5.png)
- ![alt text](image-6.png)

### AWS Services
- We can select the services by region
- We can see which services are available in which region
- Not every service is in every region

## IAM: Users and Groups
- ![alt text](image-7.png)
- ![alt text](image-9.png)
- IAM is a global service and it is not region specific
- Not the best practice to use root account
- ![alt text](image-10.png)
- ![alt text](image-11.png)
- ![alt text](image-12.png)
- ![alt text](image-13.png)
- ![alt text](image-14.png)
- ![alt text](image-15.png)
- ![alt text](image-16.png)
- ![alt text](image-17.png)
  - Users inherit permissions from their group
  - ![alt text](image-18.png)
  - We can simplify our sign-in URL using aliases
  - ![alt text](image-19.png)
  - ![alt text](image-20.png)

## Multi-Session Support
- Multi-session support allows using multiple AWS accounts in the same browser at once.
- You can enable it, add sessions, and log in with different account IDs side by side.
- Each session is isolated, keeping identities and resources separate.
- Actions in one account (e.g., creating an EBS volume) won’t appear in another.
- It removes the need for multiple browsers or incognito windows.
- Useful for efficiently managing multiple AWS accounts at scale.

## IAM Policies

- IAM policies define permissions and can be attached to users or groups.
- Group-level policies apply to all members, while different groups can have different policies.
- Users can also have inline policies that apply only to them, even if they’re not in a group.
- A user can inherit multiple policies if they belong to multiple groups (e.g., Charles gets both Developer and Audit policies).
- IAM policies are JSON documents with fields like Version, optional ID, and one or more Statements.
- Each statement includes Sid (optional ID), Effect (Allow/Deny), Principal (who it applies to), and Actions (allowed/denied API calls).
- ![alt text](image-21.png)
- ![alt text](image-22.png)

### IAM Permissions
- IAM permissions decide **what actions a user can perform in AWS** (like view, create, or delete resources).

- You can assign permissions in 2 main ways:
  - **Groups** → One policy applied to many users (easy management)
  - **Direct (user policies)** → Specific permissions for one user

- If a user is **removed from a group**, they immediately **lose those permissions**.
- ![alt text](image-23.png)

- A user can have **multiple permissions at the same time**:
  - From groups
  - From direct policies  
  👉 Final access = **combined permissions**

- Policies are written in JSON but think of them simply as:
  - **Effect** → Allow or Deny
  - **Action** → What you can do (e.g., ListUsers)
  - **Resource** → Where you can do it

- `*` means **everything**:
  - `Action: *` → all actions
  - `Resource: *` → all resources  
  👉 This is basically **admin access**

- Example:
  - **Read-only policy** → can view things but not create/delete
  - **Admin policy** → can do anything

- Key idea:  
  👉 Give **only the permissions needed** (principle of least privilege)

- ![alt text](image-24.png)

## IAM Password Policy
- To protect AWS users and accounts, there are **2 main security layers**:
  👉 **Password Policy** + **Multi-Factor Authentication (MFA)**

- **Password Policy** makes passwords stronger by:
  - Setting minimum length
  - Requiring uppercase, lowercase, numbers, symbols
  - Forcing password changes after some time (e.g., 90 days)
  - Preventing reuse of old passwords  
  👉 Helps protect against hacking (brute force attacks)

- **MFA (Multi-Factor Authentication)** adds extra security:
  - You log in with:
    1. Something you **know** (password)
    2. Something you **have** (device/code)
  👉 Even if password is stolen, account stays safe

- Example:
  - Alice logs in using password + code from her phone
  - Hacker needs BOTH → much harder to break in

- Types of MFA devices in AWS:
  - **Virtual MFA** → Apps like Google Authenticator or Authy (most common)
  - **Hardware key (U2F)** → Physical devices like YubiKey
  - **Hardware tokens** → Key fob devices (less common)

- Key idea:
  👉 Always enable MFA (especially for root & admin users)  
  👉 Password alone is **not enough for security**

  - ![alt text](image-25.png)
  - ![alt text](image-26.png)

## Setting a Password Policy

- You can set a **Password Policy** in IAM (Account Settings):
  - Define minimum length
  - Require uppercase, lowercase, numbers, symbols
  - Force password expiry (e.g., every 90 days)
  - Allow users to change passwords
  - Prevent reuse of old passwords  
  👉 This strengthens basic account security

- The **most important account to protect is the Root Account**.

- Add **MFA (Multi-Factor Authentication)** for extra security:
  - Go to **Security Credentials → Assign MFA**
  - Choose device type (usually an authenticator app)

- **How MFA setup works**:
  - Scan a QR code using an app (like Authenticator)
  - App generates changing codes (every ~30 sec)
  - Enter 2 codes to confirm setup

- **How login works after MFA**:
  - Step 1: Enter password  
  - Step 2: Enter MFA code from your phone  
  👉 Only then you can access AWS

- ⚠️ Important:
  - If you lose your MFA device, you may **lose access to your account**
  - Always keep backup or be careful

- Key idea:
  👉 Password policy = basic protection  
  👉 MFA = strong protection (must enable, especially for root)

- ![alt text](image-27.png)

# AWS Access Keys, CLI and SDK

- There are **3 ways to access AWS**:
  1. **Management Console (UI)** → Web interface (login with username/password + MFA)
  2. **CLI (Command Line Interface)** → Use terminal commands
  3. **SDK (Software Development Kit)** → Use code inside applications

- **CLI & SDK require Access Keys**:
  - **Access Key ID** → like a username
  - **Secret Access Key** → like a password  
  👉 Used to authenticate programmatically

- **Important security rule**:
  👉 Never share your access keys (they give full access like a password)

- **CLI (Command Line Interface)**:
  - Run commands like `aws s3 cp ...`
  - Useful for automation, scripts, and faster operations
  - Directly interacts with AWS APIs

- **SDK (Software Development Kit)**:
  - Libraries for programming languages (Python, Java, Node.js, etc.)
  - Used to build apps that interact with AWS

- Example:
  - Console → manual work (clicking)
  - CLI → automation via commands
  - SDK → automation via code

- Key idea:
  👉 Console = easy/manual  
  👉 CLI = powerful/automation  
  👉 SDK = programmatic integration

- ![alt text](image-28.png)
- We can use the AWS CLI to automate a lot of our tasks
- Mostly people use CLI instead of Web UI interface
- ![alt text](image-30.png)


# AWS CLI

- **Access keys are created from IAM user → Security Credentials → Create Access Key**
  👉 You get:
  - Access Key ID (like username)
  - Secret Access Key (like password)

- ⚠️ Important:
  - You can see/download the secret key **only once**
  - Keep it safe and never share it

- After creating keys, configure CLI using:
  - `aws configure`
  - Enter access key, secret key, region, and output format

- Once configured, you can run commands like:
  - `aws iam list-users` → shows users in your account

- **Permissions still apply**:
  - If your user loses permissions (e.g., removed from admin group)
  - CLI commands will also fail (just like the console)

- Key idea:
  👉 Access keys = login for CLI/SDK  
  👉 Same permissions everywhere (Console = CLI = SDK)

- ![alt text](image-31.png)
- ![alt text](image-32.png)
- ![alt text](image-33.png)


# AWS CloudShell

- **AWS CloudShell** is a **browser-based terminal** inside AWS  
  👉 No need to install CLI on your computer

- It already has **AWS CLI pre-installed and configured**
  👉 Uses your logged-in account automatically (no access keys needed)

- You can run commands like:
  - `aws iam list-users`
  👉 Works just like your local terminal

- **Region behavior**:
  - Default region = the region selected in AWS Console

- **File storage**:
  - You can create files (e.g., text files)
  - Files are **saved and persist** even after restarting CloudShell

- **Extra features**:
  - Upload/download files
  - Multiple tabs or split terminals
  - Customize theme, font size, etc.

- ⚠️ Note:
  - CloudShell is **not available in all regions**

- Key idea:
  👉 CloudShell = easiest way to use CLI (no setup)  
  👉 Local CLI = more control, but needs setup

- ![alt text](image-34.png)

# IAM Roles

- **IAM Roles** are used to give **permissions to AWS services (not humans)**

- Think of a role like a **temporary identity with permissions**
  👉 Similar to a user, but used by services like EC2, Lambda, etc.

- Example:
  - An **EC2 instance (virtual server)** wants to access AWS (e.g., S3)
  - It cannot do anything by default ❌
  - You attach an **IAM Role** with permissions ✅
  👉 Now EC2 can perform actions based on that role

- How it works:
  - Create a role with specific permissions
  - Attach it to a service (like EC2)
  - Service “assumes” the role to make API calls

- Common use cases:
  - EC2 accessing S3
  - Lambda calling other AWS services
  - CloudFormation creating resources

- Key idea:
  👉 Users = for people  
  👉 Roles = for AWS services  
  👉 Roles allow secure, permission-based access without sharing credentials

- ![alt text](image-35.png)

## More on IAM Roles

- You can create IAM Roles from **IAM → Roles → Create Role**

- Choose **“AWS Service”** as the role type  
  👉 This means the role will be used by an AWS service (not a user)

- Select the service (e.g., **EC2**)  
  👉 This defines **who can use the role**

- Attach a **policy (permissions)** to the role  
  👉 Example: IAM ReadOnly → allows only viewing IAM data

- Give the role a name (e.g., DemoRoleForEC2) and create it

- **Trusted entity**:
  👉 Defines which service can assume the role (e.g., EC2)

- Important:
  - Role itself does nothing until attached to a service
  - Example: Attach role to EC2 → EC2 gets those permissions

- Key idea:
  👉 Create role → attach permissions → assign to service  
  👉 Service uses role to perform actions securely

- ![alt text](image-36.png)
- ![alt text](image-37.png)

## IAM Security Tools

- IAM provides **2 important security tools** to monitor and improve access

- **1. IAM Credentials Report (Account-level)**:
  - Shows all users in your account
  - Includes details like passwords, access keys, MFA status
  👉 Helps you audit overall account security

- **2. IAM Access Advisor (User-level)**:
  - Shows which AWS services a user can access
  - Also shows **when those services were last used**
  👉 Helps identify unused permissions

- Why this matters:
  👉 You can remove unnecessary permissions  
  👉 Follow **Principle of Least Privilege** (give only what’s needed)

- Key idea:
  👉 Credentials Report = security overview  
  👉 Access Advisor = usage-based permission cleanup

- ![alt text](image-38.png)

- **Credentials Report**:
  - Downloaded as a CSV from IAM
  - Shows details for all users (including root):
    - Password status (enabled, last used, last changed)
    - MFA status (enabled or not)
    - Access keys (created, last used, rotated)
  👉 Helps identify weak security (no MFA, unused accounts, old passwords)

- Example insights:
  - Root has MFA enabled ✅
  - User may not have MFA ❌
  - Some users may have access keys but rarely use them

- **Access Advisor**:
  - Found inside a specific user
  - Shows:
    - Which AWS services the user accessed
    - When they last used them

- Why it's useful:
  - Identify **unused permissions**
  - Remove unnecessary access
  👉 Improves security using least privilege

- Key idea:
  👉 Credentials Report = find security risks  
  👉 Access Advisor = clean up permissions based on usage


- ![alt text](image-39.png)

- **Credentials Report (Account-level)**:
  - Download as a CSV from IAM
  - Shows all users (including root) with details like:
    - Password usage & last change
    - MFA status (enabled/disabled)
    - Access keys (created, last used, rotated)
  👉 Helps find security risks (no MFA, old passwords, unused users)

- Example:
  - Root account may have MFA enabled ✅
  - Other users might not ❌ → security risk

- **Access Advisor (User-level)**:
  - Shows which AWS services a user has accessed
  - Also shows **last accessed time**

- Why Access Advisor is useful:
  - Identify services the user **never used**
  - Remove unnecessary permissions
  👉 Follow **least privilege principle**

- Extra insight:
  - It also shows **which policy gave access** to a service

- Key idea:
  👉 Credentials Report = audit security  
  👉 Access Advisor = optimize permissions

- ![alt text](image-40.png)

## IAM Best Practices
- **Do NOT use the root account daily**  
  👉 Use it only for initial setup or critical tasks

- **One user = one person**  
  👉 Never share accounts; create separate users for each person

- **Use Groups for permissions**  
  👉 Assign permissions to groups, then add users → easier management

- **Enable strong security**:
  - Strong password policy
  - Mandatory MFA (especially for admin users)  
  👉 Protects against hacks

- **Use IAM Roles for AWS services**  
  👉 Example: EC2 accessing S3 without using access keys

- **Use Access Keys safely** (for AWS CLI/SDK):
  - Treat them like passwords
  - Never share them

- **Regularly review permissions**:
  - Use Credentials Report → check security status
  - Use Access Advisor → remove unused permissions

- 🚨 Critical rule:
  👉 Never share IAM users, passwords, or access keys

- Key idea:
  👉 Secure access + least privilege = safe AWS usage

- **Shared Responsibility Model** =  
  👉 Defines what **AWS secures** vs what **YOU secure**

- **AWS is responsible for (Security OF the cloud)**:
  - Physical infrastructure (data centers, hardware)
  - Global network & underlying services
  - Service security, patching, and compliance

- **YOU are responsible for (Security IN the cloud)**:
  - Creating users, groups, roles, and policies
  - Managing and assigning permissions
  - Enabling MFA and strong security practices
  - Rotating passwords and access keys
  - Monitoring access and reviewing permissions

- Example (IAM):
  - AWS provides IAM service ✅
  - YOU decide who gets access and how ❗

- Key idea:
  👉 AWS secures the **cloud itself**  
  👉 You secure **how you use the cloud**

- ![alt text](image-41.png)

- ![alt text](image-42.png)

# EC2 Fundamentals

- Amazon EC2 (Elastic Compute Cloud) lets you rent virtual computers (called instances) on AWS to run websites or apps.
- You can choose everything for your machine: OS (Linux/Windows/macOS), CPU, RAM, storage, and network.
- EC2 also includes features like load balancing, auto-scaling, and storage (EBS) to manage apps easily.
- A Security Group acts like a firewall to control who can access your instance.
User Data script runs once when the instance starts, used to automatically install software or set things up.
```shell
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Hello from EC2" > /var/www/html/index.html
```

![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-45.png)
![alt text](image-46.png)
![alt text](image-47.png)
- If we stop an EC2 instance and start it again, it might change its public IP, but private IP remains intact
- AWS has different EC2 instance types, each designed for specific needs like general use, heavy computing, memory-heavy apps, or storage-heavy tasks.
- Instance names (like m5.2xlarge) tell you: type (M), generation (5), and size (2xlarge).
- General purpose (T, M) → balanced (used for normal apps like web servers).
- Compute optimized (C) → strong CPU (good for ML, gaming, heavy processing).
- Memory (R, X) and Storage (I, D, H) → for big data, databases, and high-speed storage needs.
- Pick instance type based on what your app needs most → CPU, RAM, or Storage.
- ![alt text](image-48.png)
- ![alt text](image-49.png)
- ![alt text](image-50.png)
- ![alt text](image-51.png)

# Security Groups in AWS(Similar to NSG in Azure)
- A Security Group in AWS is like a firewall that controls what traffic can go into and out of your EC2 instance.
- It only has allow rules (no deny), and you can allow traffic by IP address or other security groups.
- By default: incoming traffic is blocked, but outgoing traffic is allowed.
- If your app doesn’t load → usually a security group issue (port not allowed).
- Important ports: 22 (SSH), 80 (HTTP), 443 (HTTPS), 3389 (Windows RDP).
- Example Security Group Rule (Allow HTTP)
```shell
Type: HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0

```
- This means:
“Allow anyone on the internet to access my server via a web browser.”
![alt text](image-52.png)
![alt text](image-53.png)
![alt text](image-54.png)
![alt text](image-55.png)
![alt text](image-56.png)
![alt text](image-57.png)

- You explored Security Groups in AWS console, where you can see and edit rules for your EC2 instance.
- Inbound rules control access into your server (like SSH on port 22 and HTTP on port 80).
- If you remove HTTP (port 80), your website stops loading → timeout happens.
- Timeout = security group problem (port not allowed), so always check rules first.
- One EC2 can have multiple security groups, and one security group can be used by many instances.

```shell
# Example: Allow HTTP access (fix website issue)
Type: HTTP
Port: 80
Source: 0.0.0.0/0

```

# Differences with NSG

### Stateful behavior
Security Groups → always stateful (return traffic automatically allowed)
NSG → also stateful, but rules feel more explicit and structured
### Allow vs Deny rules
Security Groups → only allow rules (no deny)
NSG → both allow and deny rules
### Attachment level
Security Groups → attached directly to instances (ENI)
NSG → can be attached to subnet OR NIC
### Default behavior
Security Groups → inbound blocked, outbound allowed
NSG → has default rules (allow/deny mix) already defined

- To connect to your EC2 (Linux server), you mainly use SSH (Secure Shell) — it lets you control the server from your computer.
- On Mac/Linux/Windows 10+, you can use built-in SSH; on older Windows, use PuTTY (same purpose).
- There’s an easier option called EC2 Instance Connect, which works from your browser (no setup needed).
- SSH issues are common — usually due to wrong key, command, or security group settings.
- You only need one method to work (SSH, PuTTY, or Instance Connect), not all.
- Example SSH command
```shell
ssh -i my-key.pem ec2-user@<public-ip>
```
- This connects your computer to the EC2 server securely.
- ![alt text](image-58.png)
- SSH lets you log into your EC2 server from your computer terminal and control it like you’re inside it.
- You connect using public IP + key file (.pem) and the default user (ec2-user).
- Make sure port 22 is allowed in the Security Group, or SSH won’t work.
- You must set correct permissions on the key file (chmod 400) before connecting.
- Once connected, you can run commands on the server and exit anytime.
- Example SSH Command
```shell
ssh -i EC2Tutorial.pem ec2-user@<public-ip>
```
- Fix key permission (important step)
```shell
chmod 400 EC2Tutorial.pem
```
- This makes your key secure so SSH will allow login.
- ![alt text](image-59.png)
- ![alt text](image-60.png)

# EC2 Instance Connect

- EC2 Instance Connect lets you log into your EC2 server directly from the browser (no need for SSH keys or terminal).
- AWS automatically uses a temporary key behind the scenes, so setup is easier than normal SSH.
- You still need port 22 (SSH) open in the Security Group, or it won’t connect.
- Once connected, you can run commands just like SSH (whoami, ping, etc.).
- It’s basically SSH made simpler and more beginner-friendly.

# Example commands you can run after connecting
```shell
whoami
ping google.com
```
These run directly on your EC2 server from the browser.

### EC2 Instance Connect = SSH without the headache (no key management, browser-based)

# Running AWS Commands on EC2 Instances

- EC2 instances can run AWS commands using the AWS CLI, but they need permissions (credentials).
- ❌ Don’t use aws configure with access keys on EC2 — it’s unsafe and can expose your credentials.
- ✅ Instead, attach an IAM Role to the EC2 instance — this safely gives permissions.
- Once the role is attached, AWS commands (like listing users) start working automatically.
- If permissions are removed from the role → commands fail (access denied).

![alt text](image-61.png)

# EC2 Purchase Options
AWS gives different ways to pay for EC2, depending on your usage (short-term, long-term, cheap, or dedicated).
- On-Demand → pay as you use (flexible but most expensive).
- Reserved / Savings Plans → commit for 1–3 years → big discounts (best for steady workloads).
- Spot Instances → super cheap (up to ~90% off) but can stop anytime.
Dedicated / Capacity Reservation → for special needs like compliance or guaranteed capacity.
### Easy way to remember
- On-Demand → flexible, no commitment
- Reserved / Savings → cheaper, long-term
- Spot → cheapest, but risky
- Dedicated → full control (expensive)

- ![alt text](image-62.png)
- ![alt text](image-63.png)
- ![alt text](image-64.png)
- ![alt text](image-65.png)

# EC2 Instance Storage
- **EBS (Elastic Block Store)** is a **network-attached storage** for EC2 instances, like a virtual USB drive, used to **persist data even after instance termination**.  
- Volumes are tied to a **specific Availability Zone (AZ)** and can be attached to **only one instance at a time** (at CCP level).  
- They can be **detached and reattached** to another instance quickly, useful for failover and recovery scenarios.  
- You must **provision size (GB) and IOPS in advance**, and billing is based on this provisioned capacity (can scale later).  
- EBS supports **snapshots** to move data across AZs or back up volumes.  
- **Delete on Termination**: Root volume is deleted by default, while additional volumes persist unless configured otherwise.
- ![alt text](image-66.png)
- ![alt text](image-67.png)
- ![alt text](image-68.png)

# Create an EBS volume
aws ec2 create-volume --size 20 --availability-zone us-east-1a

# Attach it to an EC2 instance
aws ec2 attach-volume --volume-id vol-123 --instance-id i-123 --device /dev/sdf

- You can view EBS volumes in the EC2 console under **Storage → Volumes**, where each instance has a **root volume** (default ~8GB) and optional additional volumes. 
- New EBS volumes must be created in the **same Availability Zone (AZ)** as the EC2 instance to be attached; otherwise, attachment is not possible. 
- Multiple EBS volumes can be attached to a single instance, but they must be **manually formatted and mounted** before use. 
- Volumes can exist independently (unattached), and can be **attached/detached dynamically** via the console or CLI.   
- When an instance is terminated, the **root volume is deleted by default**, while additional volumes persist unless configured otherwise. 
- This demonstrates the flexibility of cloud storage: **create, attach, or delete volumes instantly** as needed.

**Example (CLI):**
```bash
# Create a 2GB EBS volume in a specific AZ
aws ec2 create-volume --size 2 --availability-zone eu-west-1b

# Attach it to an instance
aws ec2 attach-volume --volume-id vol-xyz --instance-id i-xyz --device /dev/sdf

# Delete a volume
aws ec2 delete-volume --volume-id vol-xyz
```

# EBS Snapshots

- **EBS Snapshots** are point-in-time backups of EBS volumes; you don’t need to detach the volume, though it’s recommended for consistency.  
- Snapshots enable **data transfer across Availability Zones or Regions** by restoring them into new volumes elsewhere.  
- **Archive Tier** allows storing snapshots at up to ~75% lower cost, but restoration takes **24–72 hours**.  
- **Recycle Bin** protects against accidental deletion, letting you recover snapshots within a **retention window (1 day–1 year)**.  
- **Fast Snapshot Restore (FSR)** eliminates initial latency when creating volumes from snapshots, but is **expensive** and used for high-performance needs.  

**Example (CLI):**
```bash
# Create snapshot from a volume
aws ec2 create-snapshot --volume-id vol-123 --description "My backup"

# Copy snapshot to another region
aws ec2 copy-snapshot --source-region us-east-1 --source-snapshot-id snap-123 --region us-west-2

# Restore volume from snapshot
aws ec2 create-volume --snapshot-id snap-123 --availability-zone us-west-2a

```
- ![alt text](image-69.png)

- You can create an **EBS Snapshot** from a volume via EC2 → Volumes → *Create Snapshot*, and track it under the **Snapshots** section.  
- Snapshots can be **copied across regions** for disaster recovery and backup strategies.  
- You can **restore a snapshot into a new EBS volume** in a different Availability Zone, effectively duplicating data across AZs.  
- Snapshots support **storage tiers** (Standard → Archive) to reduce cost, with slower restore times (24–72 hrs).  
- **Recycle Bin** allows recovery of deleted snapshots based on a retention rule (e.g., 1 day–1 year).  
- This makes snapshots powerful for **backup, migration, and recovery workflows** in AWS.  

**Example (CLI):**
```bash
# Create snapshot
aws ec2 create-snapshot --volume-id vol-123 --description "DemoSnapshots"

# Create volume from snapshot in another AZ
aws ec2 create-volume --snapshot-id snap-123 --availability-zone eu-west-1b

# Copy snapshot to another region
aws ec2 copy-snapshot --source-region eu-west-1 --source-snapshot-id snap-123 --region us-east-1

```

# Amazon Machine Image

- **AMI (Amazon Machine Image)** is a pre-configured template used to launch EC2 instances, including OS, software, and configurations.  
- It can contain **custom apps, monitoring tools, and settings**, enabling faster boot and consistent environments.  
- AMIs can be **AWS-provided (public), user-created (custom), or from AWS Marketplace** (third-party/vendor images).  
- AMIs are **region-specific**, but can be **copied across regions** to replicate setups globally.  
- Creating an AMI involves: launch instance → customize → stop instance → create AMI (which also creates EBS snapshots).  
- You can then **launch identical EC2 instances** from the AMI across different Availability Zones or regions.  

**Example (CLI):**
```bash
# Create AMI from an EC2 instance
aws ec2 create-image --instance-id i-123 --name "MyCustomAMI"

# Launch EC2 instance from AMI
aws ec2 run-instances --image-id ami-123 --instance-type t2.micro
```

- You can **create an AMI from a configured EC2 instance** (e.g., after installing software like Apache) to reuse the same setup later. 
- After launching an instance and running **user data scripts**, wait for setup to complete before creating the AMI. 
- Creating an AMI automatically generates **EBS snapshots** and registers the image for reuse. 
- You can then **launch new EC2 instances from your custom AMI**, avoiding repeated installation steps. 
- This results in **faster boot times**, since required software is already pre-installed in the AMI. 
- Additional customization can still be done via **user data scripts** when launching from the AMI. 
**Example (CLI):**
```bash
# Create AMI from an instance
aws ec2 create-image --instance-id i-123 --name "demo-image"

# Launch instance from custom AMI
aws ec2 run-instances --image-id ami-123 --instance-type t2.micro

# Example user data (runs on launch)
#!/bin/bash
echo "Hello World" > /var/www/html/index.html
```
# EC2 Instance Store

- **EC2 Instance Store** is **physically attached storage** (local disk) on the host machine, offering **very high I/O performance and throughput**.  
- It is ideal for workloads needing **ultra-fast read/write speeds** (much higher than EBS), such as caching or temporary processing.  
- However, it is **ephemeral storage** → data is **lost if the instance is stopped or terminated**.  
- Not suitable for long-term storage; use **EBS for durability and persistence** instead.  
- Common use cases: **cache, buffers, scratch data, temporary files**.  
- You are responsible for **backup and replication**, as hardware failure can lead to data loss.  

**Example (conceptual):**
```bash
# Instance Store is automatically available on supported instance types (e.g., i3)
# Mounting local disk (Linux example)
lsblk
sudo mkfs -t ext4 /dev/nvme0n1
sudo mount /dev/nvme0n1 /mnt/instance-store
```
- ![alt text](image-70.png)

# EBS Volume Types

- **EBS volumes come in 6 types**:  
  - **General Purpose SSD** → gp2, gp3 (balanced cost & performance)  
  - **Provisioned IOPS SSD** → io1, io2 (high-performance, low-latency)  
  - **HDD** → st1 (throughput optimized), sc1 (cold storage)

- **gp3 vs gp2**: gp3 is newer with **independent control of IOPS & throughput**, while gp2 links performance to volume size.

- **io1/io2** are best for **critical workloads (e.g., databases)** requiring consistent high IOPS; io2 offers even higher limits and lower latency. 

- **st1/sc1 (HDD)** are cheaper options:  
  - st1 → big data, logs (high throughput)  
  - sc1 → archive/rarely accessed data (lowest cost) 

- Only **gp2, gp3, io1, io2** can be used as **boot/root volumes**. 

- Key factors when choosing: **size (GB), IOPS, throughput, and cost vs performance trade-off**. 

**Example (CLI):**
```bash
# Create a gp3 volume with custom IOPS & throughput
aws ec2 create-volume \
  --size 20 \
  --volume-type gp3 \
  --iops 3000 \
  --throughput 125 \
  --availability-zone us-east-1a

# Create high-performance io2 volume
aws ec2 create-volume \
  --size 100 \
  --volume-type io2 \
  --iops 20000 \
  --availability-zone us-east-1a

```
- ![alt text](image-71.png)
- ![alt text](image-72.png)


# EBS Multi-Attach

- **EBS Multi-Attach** allows a single EBS volume to be **attached to multiple EC2 instances simultaneously** (same AZ).  
- Supported only for **high-performance SSD volumes (io1, io2)**.  
- All attached instances get **read & write access at the same time**, enabling shared storage.  
- Useful for **clustered applications** (e.g., distributed systems, shared databases) requiring high availability.  
- Limit: up to **16 EC2 instances** can attach to the same volume.  
- Requires a **cluster-aware file system** to safely handle concurrent writes.  

**Example (conceptual CLI):**
```bash
# Create io2 volume with Multi-Attach enabled
aws ec2 create-volume \
  --volume-type io2 \
  --multi-attach-enabled \
  --size 100 \
  --availability-zone us-east-1a

# Attach to multiple instances
aws ec2 attach-volume --volume-id vol-123 --instance-id i-1 --device /dev/sdf
aws ec2 attach-volume --volume-id vol-123 --instance-id i-2 --device /dev/sdf

```
- ![alt text](image-74.png)

# Elastic File System

- **Amazon EFS (Elastic File System)** is a **managed NFS-based shared file system** that can be mounted on **multiple EC2 instances across different Availability Zones**. 
- It is **highly available, scalable (up to petabytes), and pay-as-you-go**, with no need to provision storage in advance. 
- Common use cases: **content management, web hosting, data sharing, WordPress**.  
- Supports **performance modes** (General Purpose, Max I/O) and **throughput modes** (Bursting, Provisioned, Elastic). 
- Offers **storage tiers** (Standard, IA, Archive) with lifecycle policies for cost savings (up to ~90%). 
- Works only with **Linux**, supports encryption (KMS), and is **more expensive than EBS (~3x)**.

**Example (Linux mount):**
```bash
# Install NFS client and mount EFS
sudo yum install -y nfs-utils
sudo mount -t nfs4 fs-123:/ /mnt/efs

# Persist mount across reboots
echo "fs-123:/ /mnt/efs nfs4 defaults,_netdev 0 0" >> /etc/fstab
```

- **EBS Multi-Attach** lets you attach one volume to multiple EC2 instances, but only within the **same AZ**, with a limit of **16 instances**, and requires a **cluster-aware filesystem**.  

- In contrast, **EFS** is simpler, works **across multiple AZs**, auto-scales, and supports thousands of instances—making it better for most shared storage use cases. 

- ![alt text](image-75.png) 
- ![alt text](image-76.png)
- ![alt text](image-78.png)


- Create an **EFS file system** (regional for multi-AZ, one-zone for cheaper dev use), enable **encryption, backups, and lifecycle policies** for cost optimization. 
- Choose **throughput mode**: Elastic (auto-scale, recommended), Bursting (based on size), or Provisioned (fixed throughput).  
- Configure **network + security groups (NFS port 2049)** and mount targets across AZs.
- Launch EC2 instances in different AZs and **attach/mount the same EFS** using `/mnt/efs/...`.
- Files created in one instance are **instantly visible in others**, proving shared storage across AZs.
- EFS is **pay-per-use, auto-scaled, and ideal for shared storage**, but should be cleaned up (instances + file system) after use. 

**Example (Linux usage):**
```bash
# Write file to EFS
echo "hello world" > /mnt/efs/fs1/hello.txt

# Read from another instance
cat /mnt/efs/fs1/hello.txt
```

# EBS vs EFS
- **EBS**: Attached to **one EC2 instance (per AZ)**, high-performance block storage; can move across AZs via **snapshots**, and supports configurable IOPS (gp3/io1).  
- **EFS**: **Shared file system (NFS)** mounted on **multiple instances across AZs**, auto-scaled, ideal for shared workloads like WordPress (Linux only).  
- EBS is **cheaper and faster for single-instance use**, while EFS is **more expensive but scalable and multi-AZ**.  
- EBS root volumes are **deleted on instance termination by default**, while EFS persists independently.  
- **Instance Store**: local disk with **very high performance but ephemeral** (data lost on stop/terminate).  

- ![alt text](image-79.png)
- ![alt text](image-80.png)


# AWS Fundamentals: ELB + ASG

- **Scalability** = ability to handle more load:  
  - **Vertical scaling** → increase instance size (e.g., t2.micro → t2.large)  
  - **Horizontal scaling** → add more instances (scale out/in) 

- **High Availability (HA)** = running across **multiple AZs/data centers** to survive failures.

- Horizontal scaling often enables HA, but they are **different concepts** (capacity vs fault tolerance). 

- In AWS:  
  - Vertical → bigger EC2 instance  
  - Horizontal → Auto Scaling / Load Balancer  
  - HA → multi-AZ deployment 

- ![alt text](image-81.png)

# Elastic Load Balancing Overview

- **Load Balancer** distributes incoming traffic across multiple EC2 instances, providing a **single entry point** and improving scalability. 
- It enables **high availability, fault tolerance, health checks**, and features like SSL termination and sticky sessions.  
- AWS offers managed load balancers: **ALB (HTTP/HTTPS), NLB (TCP/UDP), GWLB**, while CLB is legacy. 
- Load balancer performs **health checks** and routes traffic only to healthy instances.
- Security: users access the **load balancer**, while EC2 instances accept traffic **only from the load balancer’s security group**. 
- ![alt text](image-82.png)
- ![alt text](image-83.png)
- ![alt text](image-84.png)
- ![alt text](image-85.png)
- ![alt text](image-87.png)
- ![alt text](image-93.png)


# Application Load Balancer
- **Application Load Balancer (ALB)** is a **Layer 7 (HTTP/HTTPS)** load balancer that routes traffic based on **URL paths, hostnames, headers, or query params**. 
- It uses **target groups** (EC2, ECS, Lambda, or IPs) and supports **microservices & container-based apps**. 
- ![alt text](image-88.png)
- ![alt text](image-89.png)
- ![alt text](image-90.png)
- Enables **advanced routing** (e.g., `/users` → one service, `/search` → another) and supports **HTTP/2, WebSockets, redirects**.
- Performs **health checks at target group level** and routes only to healthy targets. 
- ![alt text](image-91.png)
- 
- Client IP is passed via headers like **X-Forwarded-For**, since traffic goes through the load balancer. 
- ![alt text](image-92.png)

- You launched **2 EC2 instances (VMs)** → each returns “Hello World”, but each has its own IP (not ideal for users). 

- Then you created an **Application Load Balancer (ALB)** → a **single public endpoint** that distributes traffic across both instances. 

- You created a **Target Group** → think of it like an Azure **backend pool** (group of VMs behind the load balancer). 

- The ALB sends requests to instances in the target group → refreshing shows traffic switching between instances (load balancing). 

- **Health checks**: if one instance stops, ALB automatically **stops sending traffic to it** (like Azure LB health probes). 

- When the instance comes back, it becomes **healthy again and receives traffic** automatically. 
## Azure → AWS Mental Map
- EC2 Instance = Azure VM
- ALB (Application Load Balancer) = Azure Application Gateway (Layer 7)
- Target Group = Backend Pool
- Health Check = Health Probe

- You **restricted access to EC2 instances** so they can only be reached via the **Load Balancer**, not directly → improves security. 

- This is done using **Security Groups**:  
  EC2 allows traffic **only from the Load Balancer’s security group** (not from the internet).

- You created **ALB rules (listener rules)** → route traffic based on conditions like **URL path (/error)**.  

- Based on rules, ALB can:  
  → forward to target groups  
  → redirect  
  → return fixed responses (e.g., 404 error) 

- Rules have **priority** → if multiple match, highest priority wins. 

### Azure → AWS Mental Map
- Security Group (EC2) = Azure NSG (Network Security Group)
- Restricting EC2 to LB only = Private backend behind App Gateway
- ALB Listener Rules = Azure Application Gateway routing rules (path-based routing)
- ![alt text](image-94.png)
- ![alt text](image-95.png)
- ![alt text](image-96.png)
- ![alt text](image-97.png)
- ![alt text](image-98.png)
- ![alt text](image-99.png)
- ![alt text](image-101.png)
- ![alt text](image-102.png)
- ![alt text](image-103.png)


# Network Load Balancer

# AWS Network Load Balancer (NLB) — Beginner Friendly Summary

## What is a Network Load Balancer?

A **Network Load Balancer (NLB)** is an AWS load balancer that works at:

- **Layer 4** of the OSI model
- Handles:
  - **TCP**
  - **UDP**
  - (and TLS in practice)

Think of it as:

> “A super fast traffic forwarder for raw network traffic.”

Unlike an Application Load Balancer (ALB), it does **not understand HTTP deeply**.  
It mainly routes packets based on ports and protocols.

---

# Key Exam Points

## 1. NLB Works at Layer 4

| Load Balancer | Layer | Protocols |
|---|---|---|
| ALB | Layer 7 | HTTP / HTTPS |
| NLB | Layer 4 | TCP / UDP |

### Easy Memory Trick

- **HTTP/HTTPS → ALB**
- **TCP/UDP → NLB**

If the exam mentions:

- UDP traffic
- gaming
- VoIP
- DNS
- raw TCP sockets

→ Think **NLB**

---

# 2. NLB is Extremely Fast

NLB is designed for:

- **millions of requests per second**
- **ultra-low latency**
- high-performance workloads

### Typical Use Cases

- Gaming servers
- Financial trading systems
- Real-time applications
- IoT systems
- Video streaming
- DNS services

---

# 3. NLB Supports Static IP Addresses

This is one of the MOST IMPORTANT differences.

## ALB Problem

Application Load Balancer IPs can change.

## NLB Advantage

NLB provides:

- **1 static IP per Availability Zone**
- can attach **Elastic IPs**

### Why this matters

Some clients/firewalls only allow traffic from fixed IPs.

Example:

```text
Client Firewall allows:
52.10.10.10
52.10.10.11
```

You can give these static IPs using NLB.

---

# IMPORTANT EXAM TRIGGERS

If you see these words:

- Static IP
- Elastic IP
- UDP
- Extreme performance
- Ultra low latency
- Millions of requests

→ Answer is usually **Network Load Balancer**

---

# Architecture Flow

```text
Client
   ↓
Network Load Balancer
   ↓
Target Group
   ↓
EC2 / IPs / ALB
```

Just like ALB, NLB forwards traffic to **target groups**.

---

# 4. NLB Target Groups

NLB can send traffic to:

## A. EC2 Instances

Example:

```text
NLB → EC2 instances
```

Very common setup.

---

## B. IP Addresses (Very Important)

NLB can register:

- private IPs
- on-premise servers
- external systems

### Example

```text
NLB
 ├── EC2 private IP
 └── On-prem server private IP
```

This allows hybrid cloud architectures.

---

# 5. NLB in Front of ALB

Very important architecture pattern.

```text
Client
   ↓
NLB (static IP)
   ↓
ALB (HTTP routing rules)
   ↓
EC2
```

## Why use this?

You get:

### From NLB

- Static IPs
- high performance

### From ALB

- HTTP routing
- path-based routing
- host-based routing
- cookies
- advanced Layer 7 features

This is a valid AWS architecture.

---

# 6. Health Checks

NLB supports health checks using:

- TCP
- HTTP
- HTTPS

## Example

If backend app exposes:

```text
/health
```

You can configure:

```text
Protocol: HTTP
Path: /health
```

Even though NLB itself is Layer 4.

---

# Simple Real-World Example

## Gaming Server

Requirements:

- UDP traffic
- ultra-low latency
- millions of players

Best choice:

→ **NLB**

---

# Another Real Example

## Banking App Firewall Restriction

Requirement:

> “Partner bank only allows fixed IPs.”

Best choice:

→ **NLB with Elastic IPs**

---

# Quick Comparison: ALB vs NLB

| Feature | ALB | NLB |
|---|---|---|
| OSI Layer | Layer 7 | Layer 4 |
| Protocols | HTTP/HTTPS | TCP/UDP |
| Static IP | No | Yes |
| Elastic IP | No | Yes |
| Ultra High Performance | Moderate | Excellent |
| Path-based Routing | Yes | No |
| Host-based Routing | Yes | No |
| WebSocket Support | Yes | Yes |
| Best For | Web Apps | Network Apps |

---

# Mental Map (Azure → AWS)

| AWS | Azure Equivalent |
|---|---|
| ALB | Application Gateway |
| NLB | Azure Load Balancer |

Azure Load Balancer is also:

- Layer 4
- TCP/UDP
- high performance
- static IP based

So mentally:

> AWS NLB ≈ Azure Load Balancer

---

# Super Important Exam Scenarios

## Scenario 1

> Need UDP support

✅ NLB

---

## Scenario 2

> Need static public IPs

✅ NLB

---

## Scenario 3

> Need path-based routing like `/api`

✅ ALB

---

## Scenario 4

> Millions of low latency TCP connections

✅ NLB

---

## Scenario 5

> Need both static IP + HTTP routing

✅ NLB in front of ALB

---

# Final Revision Notes

## Remember This

### Use ALB when:

- HTTP/HTTPS
- web apps
- intelligent routing

### Use NLB when:

- TCP/UDP
- static IP
- extreme performance
- ultra-low latency

---

# One-Line Memory Shortcut

> **“ALB understands web traffic, NLB moves raw network traffic very fast.”**

- ![alt text](image-105.png)

# AWS Network Load Balancer (NLB) Hands-On Summary

Source: :contentReference[oaicite:0]{index=0}

# Creating an NLB

## Steps

1. Create a **Network Load Balancer**
   - Name: `DemoNLB`
   - Type: Internet-facing
   - IP type: IPv4

2. Select:
   - VPC
   - Multiple Availability Zones (AZs)

3. Important:
   - Each AZ gets:
     - one fixed IPv4 address
   - Can optionally assign:
     - **Elastic IPs**

---

# Security Group for NLB

Create security group:

```text
demo-sg-nlb
```

Inbound rule:

```text
HTTP (Port 80) from Anywhere
```

Attach this SG to the NLB.

---

# Listener Configuration

Listener example:

```text
Protocol: TCP
Port: 80
```

Supported protocols:

- TCP
- UDP
- TCP_UDP
- TLS

---

# Target Group Setup

Create target group:

```text
demo-tg-nlb
```

Configuration:

```text
Target Type: Instances
Protocol: TCP
Port: 80
```

Register EC2 instances as targets.

---

# Health Checks

Supported health check protocols:

- TCP
- HTTP
- HTTPS

Example:

```text
Health Check Protocol: HTTP
```

Custom settings used:

```text
Healthy Threshold: 2
Timeout: 2 sec
Interval: 5 sec
```

---

# Common Problem: Unhealthy Targets

## Cause

EC2 security group only allowed traffic from ALB SG.

Example:

```text
Allow HTTP from ALB Security Group
```

But NLB traffic was blocked.

---

# Fix

Add inbound rule to EC2 SG:

```text
Allow HTTP from demo-sg-nlb
```

After this:

- health checks pass
- instances become healthy

---

# Verification

Opening NLB DNS URL shows:

```text
Hello World from EC2
```

Refreshing changes backend IPs:

→ proves load balancing works.

---

# Important Concepts Learned

## NLB Uses

- TCP/UDP traffic
- high performance
- static IP support
- low latency

---

# Security Group Flow

```text
Client
  ↓
NLB Security Group
  ↓
EC2 Security Group
```

EC2 SG must explicitly allow traffic from NLB SG.

---

# Cleanup (to avoid AWS charges)

Delete:

- NLB
- Target Group
- Optional: NLB Security Group

---

# Exam Tips

## If targets are unhealthy:

Check:

- EC2 security group rules
- Health check configuration
- Listener ports

---

# Key Memory Shortcut

> NLB can reach EC2 only if EC2 security groups allow traffic from the NLB security group.

# Gateway Load Balancer

# What is Gateway Load Balancer?

Gateway Load Balancer (GWLB) is used to:

- deploy
- scale
- manage

third-party **network virtual appliances** in AWS.

Examples:

- Firewalls
- Intrusion Detection/Prevention Systems (IDS/IPS)
- Deep Packet Inspection systems
- Traffic analyzers

---

# Main Purpose

GWLB forces all network traffic to pass through security appliances before reaching applications.

---

# Traffic Flow

```text
User
  ↓
Gateway Load Balancer
  ↓
Security Appliances
(Firewall / IDS / IPS)
  ↓
Gateway Load Balancer
  ↓
Application
```

The appliances can:

- inspect traffic
- allow traffic
- block/drop traffic

---

# Key Idea

Without GWLB:

```text
User → ALB → App
```

With GWLB:

```text
User → GWLB → Security Appliances → App
```

---

# OSI Layer

GWLB works at:

| Load Balancer | Layer |
|---|---|
| ALB | Layer 7 |
| NLB | Layer 4 |
| GWLB | Layer 3 |

GWLB works with:

- IP packets
- network-level traffic

---

# Two Main Functions

## 1. Gateway

Acts as a single entry/exit point for traffic.

---

## 2. Load Balancer

Distributes traffic across multiple security appliances.

---

# Route Tables

GWLB works by updating VPC route tables so traffic automatically flows through the GWLB.

(Advanced networking concept — high-level understanding is enough for exams.)

---

# Target Groups

GWLB target groups can contain:

## EC2 Instances

```text
Firewall EC2 instances
```

---

## Private IP Addresses

Useful for:

- on-prem appliances
- hybrid cloud setups

---

# Important Exam Keyword

If you see:

```text
GENEVE protocol
Port 6081
```

→ Think:

✅ Gateway Load Balancer

---

# When to Use GWLB

Use GWLB when you need:

- centralized traffic inspection
- firewalls
- IDS/IPS
- packet inspection
- network security appliances

---

# Mental Model

> GWLB = “Traffic inspection checkpoint before applications.”

---

# Quick Comparison

| Feature | ALB | NLB | GWLB |
|---|---|---|---|
| Layer | 7 | 4 | 3 |
| Traffic Type | HTTP/HTTPS | TCP/UDP | IP Packets |
| Main Purpose | Web routing | Fast networking | Security inspection |
| Static IP | No | Yes | No |
| Security Appliances | No | No | Yes |

---

# Key Memory Shortcut

> ALB routes web traffic, NLB routes network traffic, GWLB inspects network traffic.

- ![alt text](image-106.png)

- ![alt text](image-107.png)

# AWS Sticky Sessions

# What are Sticky Sessions?

Sticky sessions ensure:

> A client keeps connecting to the same backend instance.

---

# Normal Load Balancer Behavior

Without stickiness:

```text
Request 1 → EC2-1
Request 2 → EC2-2
Request 3 → EC2-3
```

Traffic is evenly distributed.

---

# Sticky Session Behavior

With stickiness:

```text
Client A → EC2-1
Client A again → EC2-1
```

Same client keeps hitting the same server.

---

# Supported Load Balancers

Sticky sessions work with:

- Classic Load Balancer (CLB)
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)

---

# How Sticky Sessions Work

Load balancer sends a cookie to the client.

Client sends the cookie back in future requests.

The load balancer uses that cookie to route traffic to the same backend instance.

---

# Why Use Sticky Sessions?

Useful when session data is stored locally on EC2.

Examples:

- User login sessions
- Shopping carts
- User preferences

---

# Downside

Can create uneven load distribution.

Example:

```text
One "heavy" user may keep hitting the same EC2 instance.
```

---

# Types of Cookies

## 1. Application-Based Cookie

Cookie created by:

- your application
OR
- ALB

You define:

```text
Custom cookie name
```

Do NOT use reserved names:

- AWSALB
- AWSALBTG
- AWSALBAPP

---

# 2. Duration-Based Cookie

Cookie created by load balancer itself.

Cookie names:

| Load Balancer | Cookie Name |
|---|---|
| ALB | AWSALB |
| CLB | AWSELB |

Cookie expires after configured duration.

Example:

```text
1 second → 7 days
```

---

# Enabling Sticky Sessions

Go to:

```text
Target Group
 → Actions
 → Edit Attributes
 → Stickiness
```

Options:

- Enable stickiness
- Choose cookie type
- Set duration

---

# Example Configuration

```text
Stickiness: Enabled
Type: Load balancer generated cookie
Duration: 1 day
```

---

# Browser Behavior

After enabling stickiness:

- browser receives cookie
- browser sends cookie back
- load balancer routes to same EC2 instance

---

# Important Exam Points

## Sticky sessions use:

✅ Cookies

---

## Purpose:

✅ Keep user connected to same backend server

---

## Risk:

✅ Uneven traffic distribution

---

# Mental Model

> Sticky Sessions = “Remember which server this user was using.”

---

# Quick Memory Shortcut

| Feature | Sticky Sessions |
|---|---|
| Uses | Cookies |
| Goal | Same backend server |
| Benefit | Preserve session state |
| Downside | Load imbalance |

---

# One-Line Revision

> Sticky sessions use cookies to keep a user connected to the same backend EC2 instance.
- ![alt text](image-108.png)

# Cross Zone Load Balancing

# What is Cross-Zone Load Balancing?

Cross-zone load balancing allows a load balancer node in one AZ to distribute traffic to targets in ALL AZs.

---

# Example Setup

```text
AZ-1 → 2 EC2 instances
AZ-2 → 8 EC2 instances
```

Total:

```text
10 EC2 instances
```

---

# WITH Cross-Zone Load Balancing

Traffic is distributed evenly across ALL instances.

Example:

```text
Each EC2 gets 10% traffic
```

Even if instances are in different AZs.

---

# Traffic Flow

```text
Client
  ↓
Load Balancer Node
  ↓
All EC2 instances across all AZs
```

---

# WITHOUT Cross-Zone Load Balancing

Traffic stays inside the same AZ.

Example:

```text
AZ-1:
50% traffic → only 2 EC2s
= 25% each

AZ-2:
50% traffic → 8 EC2s
= 6.25% each
```

Result:

- uneven load distribution
- some instances overloaded

---

# Key Difference

| Mode | Behavior |
|---|---|
| Cross-Zone ON | Traffic shared across all AZs |
| Cross-Zone OFF | Traffic stays within same AZ |

---

# Application Load Balancer (ALB)

## Default

✅ Cross-zone load balancing = ON

---

## Charges

✅ No inter-AZ data charges

---

## Can Be Disabled?

Yes, at:

```text
Target Group → Attributes
```

---

# Network Load Balancer (NLB)

## Default

❌ Cross-zone load balancing = OFF

---

## If Enabled

⚠️ Inter-AZ data transfer charges apply

---

# Gateway Load Balancer (GWLB)

## Default

❌ Cross-zone load balancing = OFF

---

## If Enabled

⚠️ Inter-AZ charges apply

---

# Classic Load Balancer (CLB)

## Default

❌ OFF

---

## Charges

✅ No inter-AZ charges if enabled

---

# Important Exam Defaults

| Load Balancer | Cross-Zone Default |
|---|---|
| ALB | ON |
| NLB | OFF |
| GWLB | OFF |
| CLB | OFF |

---

# Important Cost Rule

| Load Balancer | Inter-AZ Charges |
|---|---|
| ALB | No |
| NLB | Yes |
| GWLB | Yes |
| CLB | No |

---

# Where to Configure

## ALB

```text
Target Group
 → Attributes
 → Cross-Zone Load Balancing
```

---

## NLB / GWLB

```text
Load Balancer
 → Attributes
 → Cross-Zone Load Balancing
```

---

# Mental Model

> Cross-zone balancing = “Any load balancer node can send traffic to any instance in any AZ.”

---

# Quick Exam Tips

## Need evenly distributed traffic across AZs?

✅ Enable cross-zone balancing

---

## Need to avoid inter-AZ charges on NLB/GWLB?

❌ Keep it disabled

---

# One-Line Revision

> Cross-zone load balancing spreads traffic evenly across instances in all Availability Zones.


- ![alt text](image-109.png)

# AWS SSL/TLS Certificates & Load Balancers 


# What is SSL/TLS?

SSL/TLS certificates encrypt traffic between:

```text
Client ↔ Load Balancer
```

This is called:

✅ In-flight encryption

---

# SSL vs TLS

| Term | Meaning |
|---|---|
| SSL | Older protocol |
| TLS | Newer, secure version |

In practice:

> People still say “SSL certificates” even though TLS is actually used.

---

# Why SSL/TLS is Important

HTTPS websites use SSL/TLS to:

- encrypt data
- protect passwords
- secure credit card information

---

# Common Certificate Authorities (CA)

Public certificates are issued by:

- Let's Encrypt
- GoDaddy
- DigiCert
- GlobalSign
- Comodo
- Symantec

---

# SSL with Load Balancers

## Traffic Flow

```text
Client
  ↓ HTTPS (encrypted)
Load Balancer
  ↓ HTTP (inside VPC)
EC2 Instance
```

---

# SSL Termination

Load balancer decrypts HTTPS traffic.

This is called:

✅ SSL/TLS Termination

Backend traffic may continue as plain HTTP inside the private VPC.

---

# AWS Certificate Manager (ACM)

AWS manages certificates using:

✅ AWS Certificate Manager (ACM)

You can:

- create certificates
- manage renewals
- upload your own certificates

---

# HTTPS Listener

To use HTTPS on a load balancer:

```text
Listener Protocol: HTTPS
```

You must attach:

✅ SSL/TLS certificate

---

# SNI (Server Name Indication)

## Problem

How can one load balancer host multiple HTTPS websites with different certificates?

Example:

```text
mycorp.com
api.mycorp.com
example.com
```

---

# Solution: SNI

Client sends hostname during SSL handshake.

Example:

```text
Client says:
"I want www.mycorp.com"
```

Load balancer selects correct SSL certificate.

---

# SNI Example

```text
Client
  ↓
ALB/NLB
  ├── SSL Cert for mycorp.com
  └── SSL Cert for example.com
```

Load balancer chooses correct cert automatically.

---

# SNI Support

| Load Balancer | SNI Support |
|---|---|
| ALB | Yes |
| NLB | Yes |
| CLB | No |

---

# Important Exam Rule

## Multiple SSL certificates on one LB?

✅ Use:

- ALB
- NLB

Because they support SNI.

---

# Classic Load Balancer (CLB)

Supports:

✅ One SSL certificate only

If multiple domains needed:

❌ Need multiple CLBs

---

# Application Load Balancer (ALB)

Supports:

✅ Multiple SSL certificates  
✅ Multiple listeners  
✅ SNI

Best for:

- HTTPS web apps
- multiple domains

---

# Network Load Balancer (NLB)

Supports:

✅ Multiple SSL certificates  
✅ SNI

Useful for:

- high-performance TLS traffic

---

# Key Terms

| Term | Meaning |
|---|---|
| HTTPS | HTTP + Encryption |
| SSL/TLS Termination | LB decrypts traffic |
| ACM | AWS Certificate Manager |
| SNI | Multiple certs on same LB |

---

# Mental Model

> SNI lets one load balancer host multiple HTTPS websites using different SSL certificates.

---

# Important Exam Triggers

| If You See | Think |
|---|---|
| Multiple SSL certs | ALB or NLB |
| HTTPS Listener | SSL certificate required |
| Certificate management | ACM |
| SSL handshake hostname selection | SNI |

---

# One-Line Revision

> SSL/TLS encrypts client traffic, and SNI allows ALB/NLB to serve multiple HTTPS certificates from one load balancer.

- ![alt text](image-110.png)
- ![alt text](image-112.png)


# Enabling SSL/TLS on ALB and NLB — Concise Summary

# Enabling HTTPS on ALB

## Add HTTPS Listener

Configuration:

```text
Protocol: HTTPS
Port: 443
```

Traffic flow:

```text
Client
  ↓ HTTPS
ALB
  ↓
Target Group
```

---

# Forwarding Rule

Example:

```text
HTTPS : 443
 → Forward to Target Group
```

---

# SSL Security Policy

Defines:

- supported SSL/TLS versions
- encryption settings
- compatibility with older clients

Usually:

```text
Use default policy
```

unless legacy compatibility is needed.

---

# SSL Certificate Sources

ALB certificates can come from:

| Source | Recommended |
|---|---|
| ACM (AWS Certificate Manager) | ✅ Yes |
| IAM | ⚠️ Older method |
| Manual Import | Supported |

---

# Manual Certificate Import

You can paste:

- private key
- certificate body
- certificate chain

AWS imports it into ACM.

---

# Enabling TLS on NLB

## Add TLS Listener

Configuration:

```text
Protocol: TLS
```

Then:

```text
Forward to Target Group
```

---

# NLB TLS Features

Supports:

- SSL/TLS certificates
- security policies
- ACM certificates
- imported certificates

---

# Advanced TLS Setting

NLB also supports:

```text
Application Layer Protocol Negotiation (ALPN)
```

Advanced TLS feature.

(Not important for most exams.)

---

# ALB vs NLB HTTPS/TLS

| Feature | ALB | NLB |
|---|---|---|
| HTTPS/TLS Listener | Yes | Yes |
| ACM Support | Yes | Yes |
| Multiple Certificates | Yes | Yes |
| SNI Support | Yes | Yes |

---

# Important Ports

| Protocol | Port |
|---|---|
| HTTP | 80 |
| HTTPS | 443 |

---

# Key Exam Points

## HTTPS Listener Requires:

✅ SSL/TLS certificate

---

## Recommended Certificate Storage:

✅ ACM

---

## Default HTTPS Port:

✅ 443

---

## Multiple SSL Certificates:

✅ Supported by ALB and NLB using SNI

---

# Mental Model

> HTTPS listener + ACM certificate = secure encrypted traffic to the load balancer.

---

# One-Line Revision

> To enable HTTPS/TLS on ALB or NLB, create a secure listener and attach an SSL/TLS certificate from ACM.


# Connection Draining

# AWS Connection Draining / Deregistration Delay — Concise Summary

# What is Connection Draining?

Connection draining allows existing requests to finish before an EC2 instance is removed from the load balancer.

---

# Different Names

| Load Balancer | Feature Name |
|---|---|
| Classic Load Balancer (CLB) | Connection Draining |
| ALB / NLB | Deregistration Delay |

---

# Main Purpose

When an instance is:

- deregistered
- shutting down
- marked unhealthy

the load balancer:

✅ stops sending NEW requests  
✅ allows EXISTING requests to complete

---

# Traffic Flow Example

## Before Draining

```text
ELB
 ├── EC2-1
 ├── EC2-2
 └── EC2-3
```

---

## EC2-1 Enters Draining State

```text
New Users
  ↓
ELB
 ├── EC2-2
 └── EC2-3
```

Existing users on EC2-1 can still finish requests.

---

# After Draining Completes

```text
EC2-1 removed safely
```

---

# Draining Time Configuration

Allowed range:

```text
1 to 3600 seconds
```

Default:

```text
300 seconds (5 minutes)
```

Disable draining:

```text
0 seconds
```

---

# Choosing Correct Value

## Short Requests

Example:

- quick API calls
- small web requests

Use:

```text
Low draining time
Example: 30 sec
```

Allows faster instance replacement.

---

# Long Requests

Example:

- file uploads
- video processing
- long-lived connections

Use:

```text
Higher draining time
```

Prevents interrupted requests.

---

# Trade-Off

| Low Value | High Value |
|---|---|
| Faster shutdown | Safer long requests |
| Risk request interruption | Slower instance removal |

---

# Important Exam Points

## During draining:

✅ Existing connections continue  
❌ New requests are sent elsewhere

---

## Default value:

✅ 300 seconds

---

## Setting value to 0:

✅ Disables connection draining

---

# Mental Model

> Connection draining = “Gracefully remove an instance without interrupting active users.”

---

# Quick Memory Shortcut

| Feature | Purpose |
|---|---|
| Connection Draining | Finish active requests before shutdown |
| Deregistration Delay | Same thing for ALB/NLB |

---

# One-Line Revision

> Connection draining lets existing requests complete while preventing new requests from reaching a deregistering EC2 instance.

- ![alt text](image-114.png)

# Auto Scaling Groups

ASG automatically adjusts the number of EC2 instances based on load.


# Main Goals

## Scale Out

Add EC2 instances when load increases.

```text
High traffic → Add servers
```

---

## Scale In

Remove EC2 instances when load decreases.

```text
Low traffic → Remove servers
```

---

# ASG Capacity Settings

| Setting | Meaning |
|---|---|
| Minimum Capacity | Minimum running instances |
| Desired Capacity | Target number of instances |
| Maximum Capacity | Maximum allowed instances |

---

# Example

```text
Min = 2
Desired = 4
Max = 7
```

ASG keeps around 4 instances but can scale between 2 and 7.

---

# ASG + Load Balancer

ASG works very well with ELB/ALB/NLB.

Benefits:

- new EC2s auto-register with load balancer
- traffic automatically distributed
- unhealthy instances replaced automatically

---

# Self-Healing Feature

If an EC2 instance becomes unhealthy:

```text
ASG terminates it
→ launches replacement instance
```

---

# Launch Template

ASG uses a Launch Template to define how EC2 instances are created.

Includes:

- AMI
- Instance type
- Security groups
- EBS volumes
- IAM role
- Key pair
- User data
- Network settings

---

# Scaling Policies

ASG can automatically scale using CloudWatch alarms.

---

# Example

## CPU Too High

```text
Average CPU > 80%
```

Trigger:

```text
Scale Out
→ Add EC2 instances
```

---

## CPU Too Low

```text
Average CPU < 20%
```

Trigger:

```text
Scale In
→ Remove EC2 instances
```

---

# CloudWatch Integration

CloudWatch monitors metrics like:

- CPU utilization
- memory
- custom metrics

Alarms trigger scaling actions.

---

# Important Exam Points

## Scale Out

✅ Add EC2 instances

---

## Scale In

✅ Remove EC2 instances

---

## ASG Replaces Unhealthy Instances

✅ Yes

---

## ASG Cost

✅ ASG itself is free  
❌ Pay only for EC2/resources

---

# Mental Model

> ASG = “Automatically add/remove EC2 servers based on demand.”

---

# Quick Architecture

```text
Users
  ↓
Load Balancer
  ↓
Auto Scaling Group
  ↓
EC2 Instances
```

---

# One-Line Revision

> Auto Scaling Groups automatically scale EC2 instances up or down based on traffic and health checks.

- ![alt text](image-115.png)
- ![alt text](image-117.png)



# Steps to Create an ASG

## 1. Create Launch Template

Launch Template defines how EC2 instances are launched.

Includes:

- AMI
- Instance type
- Security group
- Key pair
- User data
- Storage settings

Example:

```text
AMI: Amazon Linux 2
Instance Type: t2.micro
```

---

# User Data

User data script automatically installs/configures app on new EC2 instances.

Example:

```text
Install web server on boot
```

---

# 2. Create Auto Scaling Group

Example:

```text
ASG Name: Demo ASG
```

Attach:

```text
Launch Template
```

---

# 3. Configure Network

Choose:

- VPC
- Multiple AZs

Best practice:

✅ Spread instances across AZs

---

# 4. Attach Load Balancer

Attach ASG to existing target group.

Example:

```text
Target Group → demo-tg-alb
```

Result:

✅ New EC2 instances auto-register with ALB

---

# 5. Enable Health Checks

Enable:

- EC2 health checks
- Load balancer health checks

Result:

```text
Unhealthy EC2 → terminated → replaced automatically
```

---

# Capacity Settings

Example:

```text
Desired Capacity = 1
Min Capacity = 1
Max Capacity = 1
```

ASG launches 1 EC2 instance automatically.

---

# Activity History

ASG activity history shows:

- instance launches
- instance termination
- scaling events

Example:

```text
Launching a new EC2 instance
```

---

# Load Balancer Integration

Flow:

```text
Users
  ↓
ALB
  ↓
Target Group
  ↓
ASG EC2 Instances
```

New instances become healthy and receive traffic automatically.

---

# Scaling Out

Increase desired capacity:

```text
Desired Capacity:
1 → 2
```

ASG automatically launches another EC2 instance.

---

# Result

```text
ALB distributes traffic across 2 EC2 instances
```

---

# Scaling In

Decrease desired capacity:

```text
Desired Capacity:
2 → 1
```

ASG terminates one EC2 instance automatically.

---

# Important Troubleshooting Tip

If instance stays unhealthy:

Check:

- Security groups
- User data script
- Application setup

---

# Important Exam Points

## ASG automatically:

✅ launches EC2 instances  
✅ terminates EC2 instances  
✅ replaces unhealthy instances  
✅ integrates with load balancers

---

# Key Flow

```text
Desired Capacity Changes
        ↓
ASG launches/terminates EC2
        ↓
Target Group updates automatically
        ↓
ALB routes traffic
```

---

# Mental Model

> ASG automatically keeps the required number of healthy EC2 instances running.

---

# One-Line Revision

> ASG uses launch templates and scaling policies to automatically manage healthy EC2 instances behind a load balancer.

# AWS ASG Scaling Policies — Concise Summary

# Types of Scaling Policies

## 1. Target Tracking Scaling

Simplest scaling policy.

You define:

- metric
- target value

Example:

```text
CPU Utilization Target = 40%
```

ASG automatically:

- scales out if CPU > 40%
- scales in if CPU < 40%

---

# 2. Simple / Step Scaling

Uses CloudWatch alarms.

Example:

```text
CPU > 80%
→ Add 2 EC2 instances
```

```text
CPU < 20%
→ Remove 1 EC2 instance
```

---

# 3. Scheduled Scaling

Scale based on known traffic patterns.

Example:

```text
Every Friday 5 PM
→ Increase min capacity to 10
```

Useful for predictable workloads.

---

# 4. Predictive Scaling

AWS analyzes historical traffic patterns and predicts future load.

Automatically:

- forecasts demand
- schedules scaling ahead of time

Best for:

```text
Cyclical or repeating traffic patterns
```

---

# Common Scaling Metrics

## CPU Utilization

Most common metric.

```text
High CPU → Add instances
```

---

# RequestCountPerTarget

Based on ALB requests per EC2 instance.

Example:

```text
1000 requests per target
```

Useful for web applications.

---

# Network In / Out

Useful for network-heavy applications.

Example:

- uploads
- downloads
- streaming

---

# Custom CloudWatch Metrics

You can create application-specific metrics.

Example:

```text
Queue length
Active users
Transactions/sec
```

---

# Scaling Cooldown

After scaling activity:

```text
ASG waits before scaling again
```

Default cooldown:

```text
300 seconds (5 minutes)
```

Purpose:

✅ Allows metrics to stabilize  
✅ Gives new EC2 instances time to become active

---

# Cooldown Logic

```text
Scaling Event
    ↓
Cooldown Active?
    ├── Yes → Ignore scaling
    └── No → Scale
```

---

# Optimization Tip

Use preconfigured/custom AMIs.

Why?

```text
Faster EC2 startup
→ Faster scaling
→ Smaller cooldown needed
```

---

# Detailed Monitoring

Enable:

```text
1-minute CloudWatch metrics
```

Better scaling responsiveness.

---

# Important Exam Points

| Policy Type | Purpose |
|---|---|
| Target Tracking | Maintain target metric |
| Step Scaling | Scale based on alarms |
| Scheduled Scaling | Time-based scaling |
| Predictive Scaling | Forecast future demand |

---

# Important Defaults

| Setting | Default |
|---|---|
| Cooldown Period | 300 sec |

---

# Mental Model

> ASG scaling policies automatically add/remove EC2 instances based on metrics, schedules, or forecasts.

---

# One-Line Revision

> ASG scaling policies use CloudWatch metrics and alarms to automatically scale EC2 instances up or down.

- ![alt text](image-118.png)
- ![alt text](image-119.png)

# AWS ASG Automatic Scaling — Key Points



# Types of Scaling

## 1. Scheduled Scaling

Scale at predefined times.

Example:

```text
Every Saturday 5 PM
→ Increase capacity
```

Useful for predictable traffic.

---

# 2. Predictive Scaling

AWS uses ML + historical metrics to predict traffic.

Uses metrics like:

- CPU
- Network
- ALB Request Count

Good for recurring workloads.

---

# 3. Dynamic Scaling

Real-time autoscaling based on metrics.

Types:

- Target Tracking
- Simple Scaling
- Step Scaling

---

# Target Tracking Scaling (Most Important)

Example:

```text
Target CPU = 40%
```

ASG automatically:

- adds EC2s if CPU > 40%
- removes EC2s if CPU drops

Simplest autoscaling policy.

---

# Simple Scaling

Uses CloudWatch alarms.

Example:

```text
CPU > 80%
→ Add 2 EC2s
```

---

# Step Scaling

Different scaling actions for different alarm levels.

Example:

```text
CPU > 70%
→ Add 1 EC2

CPU > 90%
→ Add 5 EC2s
```

---

# Demo Flow

## Initial State

```text
Desired Capacity = 1
Max Capacity = 3
```

---

# Stress Test

CPU artificially increased to 100% using:

```bash
stress -c 4
```

---

# What Happened

```text
High CPU
→ CloudWatch Alarm triggered
→ ASG scaled from 1 → 2 → 3 instances
```

---

# Scale-In

After CPU dropped:

```text
CloudWatch AlarmLow triggered
→ ASG removed instances
```

Capacity reduced:

```text
3 → 2 → 1
```

---

# Important Concept

Target tracking automatically creates:

✅ CloudWatch alarms

Example:

```text
AlarmHigh → Scale Out
AlarmLow → Scale In
```

---

# Key Metrics Used

| Metric | Purpose |
|---|---|
| CPU Utilization | Most common |
| Request Count | Web/API traffic |
| Network In/Out | Network-heavy apps |

---

# Important Exam Points

## Target Tracking

✅ Simplest autoscaling method

---

## CloudWatch Alarms

Trigger:

- scale out
- scale in

---

## ASG reacts automatically

CloudWatch only monitors + alarms.

ASG actually launches/terminates EC2s.

---

# Mental Model

```text
CloudWatch Metrics
      ↓
Alarm Triggered
      ↓
ASG Scaling Action
      ↓
EC2 Added/Removed
```

---

# One-Line Revision

> ASG automatic scaling uses CloudWatch metrics and alarms to dynamically add or remove EC2 instances based on load.

- ![alt text](image-120.png)
- ![alt text](image-121.png)
- ASG is similar to VM Scale Sets in Azure.
- ![alt text](image-122.png)
- ![alt text](image-123.png)
- ![alt text](image-124.png)
- ![alt text](image-125.png)


# AWS ASG Instance Refresh — Key Points

# What is Instance Refresh?

Instance Refresh is an ASG feature used to:

✅ gradually replace EC2 instances  
✅ using a new Launch Template

Common use case:

- new AMI
- updated app version
- new configuration

---

# How It Works

## Before

```text
ASG
 ├── EC2 (Old Launch Template)
 ├── EC2 (Old Launch Template)
 └── EC2 (Old Launch Template)
```

---

# Start Instance Refresh

ASG gradually:

1. terminates old instances
2. launches new instances
3. uses new launch template

---

# After

```text
ASG
 ├── EC2 (New Launch Template)
 ├── EC2 (New Launch Template)
 └── EC2 (New Launch Template)
```

---

# Important Settings

## Minimum Healthy Percentage

Example:

```text
60%
```

Meaning:

```text
At least 60% instances must stay healthy during refresh
```

Prevents downtime.

---

# Warm-Up Time

Defines how long ASG waits before considering new EC2 instance healthy.

Purpose:

✅ allows app startup  
✅ allows health checks to pass

---

# Key Benefit

Safe rolling replacement of EC2 instances without manually terminating them.

---

# Important Exam Points

| Feature | Purpose |
|---|---|
| Instance Refresh | Rolling EC2 replacement |
| New Launch Template | Used for new instances |
| Min Healthy % | Prevent downtime |
| Warm-Up Time | Wait for instance readiness |

---

# Mental Model

> Instance Refresh = “Rolling deployment for EC2 instances inside ASG.”

---

# One-Line Revision

> ASG Instance Refresh gradually replaces old EC2 instances with new ones using an updated launch template.
> This is similar to rolling deployment where we replace old VMs with new VMs which have a newer version of the code.


- ![alt text](image-126.png)

# Amazon RDS

# AWS RDS — Key Points

# What is RDS?

RDS = Relational Database Service

Managed SQL database service by AWS.

---

# Supported Database Engines

- PostgreSQL
- MySQL
- MariaDB
- Oracle
- SQL Server
- IBM DB2
- Aurora (AWS proprietary DB)

---

# Why Use RDS Instead of EC2 + DB?

AWS manages:

- DB provisioning
- OS patching
- backups
- monitoring
- scaling
- maintenance

You focus only on using the database.

---

# Important Features

| Feature | Purpose |
|---|---|
| Automated Backups | Backup DB automatically |
| Point-in-Time Restore | Restore to exact timestamp |
| Read Replicas | Improve read performance |
| Multi-AZ | High availability / DR |
| Monitoring | DB performance metrics |
| Maintenance Windows | Controlled updates |
| Scaling | Increase compute/storage |

---

# Scaling Types

## Vertical Scaling

```text
Bigger DB instance type
```

Example:

```text
db.t3.micro → db.t3.large
```

---

# Horizontal Scaling

```text
Add Read Replicas
```

Improves read throughput.

---

# Storage

RDS storage uses:

✅ EBS volumes

---

# Important Limitation

❌ Cannot SSH into RDS instance

Because RDS is fully managed by AWS.

---

# RDS Storage Auto Scaling

Automatically increases storage when DB is running low on space.

---

# Example

Initial storage:

```text
20 GB
```

If storage becomes low:

```text
RDS automatically increases storage
```

No downtime/manual scaling needed.

---

# Auto Scaling Conditions

Storage auto-scales when:

- free space < 10%
- low storage lasts > 5 mins
- last scaling was > 6 hours ago

---

# Important Setting

You define:

```text
Maximum storage threshold
```

to prevent unlimited growth.

---

# Best Use Case

Useful for:

✅ unpredictable workloads

---

# Important Exam Points

| Concept | Key Point |
|---|---|
| RDS | Managed SQL DB |
| Read Replica | Read scaling |
| Multi-AZ | High availability |
| Storage Auto Scaling | Auto increase storage |
| SSH Access | Not allowed |

---

# Azure Equivalent

| AWS RDS | Azure Equivalent |
|---|---|
| RDS | Azure SQL / Azure Database Services |

---

# One-Line Revision

> RDS is AWS’s managed relational database service with automated backups, scaling, monitoring, and high availability features.


# RDS Read Replicas vs Multi-AZ — Key Points


# RDS Read Replicas

## Purpose

✅ Scale READ operations

---

# How It Works

```text
Main DB
   ↓ async replication
Read Replica(s)
```

- Up to 15 replicas
- Same AZ / Cross AZ / Cross Region

---

# Important

Replication is:

```text
Asynchronous
```

Meaning:

```text
Eventually consistent
```

Replica may briefly have stale data.

---

# Read Replica Usage

✅ SELECT queries only

Not for:

- INSERT
- UPDATE
- DELETE

---

# Common Use Case

```text
Production App → Main DB
Reporting/Analytics → Read Replica
```

Prevents reporting workload from slowing production DB.

---

# Read Replica Benefits

| Benefit | Purpose |
|---|---|
| Read Scaling | Handle more read traffic |
| Reporting | Analytics workloads |
| Cross Region Replica | DR / global reads |
| Promote Replica | Become standalone DB |

---

# Important Exam Point

Application must explicitly connect to replicas.

```text
App connection string must use Read Replica endpoint
```

---

# Networking Cost

| Replica Type | Replication Cost |
|---|---|
| Same Region | Free |
| Cross Region | Charged |

---

# RDS Multi-AZ

## Purpose

✅ High Availability / Disaster Recovery

NOT for scaling.

---

# How It Works

```text
Primary DB
   ↓ synchronous replication
Standby DB
```

- Standby exists in another AZ
- Automatic failover

---

# Important

Replication is:

```text
Synchronous
```

No stale data.

---

# Application Connection

Application connects using:

```text
Single DNS endpoint
```

AWS automatically fails over if primary DB fails.

No app changes required.

---

# Multi-AZ Failover Triggers

- AZ failure
- DB instance failure
- storage failure
- network failure

---

# Important Limitation

❌ Cannot read from standby DB

Standby is only for failover.

---

# Read Replica vs Multi-AZ

| Feature | Read Replica | Multi-AZ |
|---|---|---|
| Purpose | Read Scaling | High Availability |
| Replication | Async | Sync |
| Readable | Yes | No |
| Stale Data Possible | Yes | No |
| Failover | Manual Promotion | Automatic |
| Scaling | Yes | No |

---

# Important Exam Question

## Can Read Replica be Multi-AZ?

✅ Yes

---

# Single AZ → Multi-AZ

Conversion:

✅ Zero downtime operation

Steps:

```text
Modify DB
→ Enable Multi-AZ
```

AWS handles everything automatically.

---

# Behind the Scenes

AWS:

1. takes snapshot
2. creates standby DB
3. enables sync replication

---

# Mental Model

## Read Replica

```text
Scale Reads
```

## Multi-AZ

```text
Survive Failures
```

---

# One-Line Revision

> Read Replicas improve read scalability, while Multi-AZ provides automatic failover and high availability.


![alt text](image-127.png)

![alt text](image-128.png)

![alt text](image-129.png)

# AWS RDS Hands-On — Key Points

# Creating an RDS Database

## Steps

```text
RDS Console
→ Databases
→ Create Database
```

Use:

```text
Full Configuration
```

to see all options.

---

# Supported Engines

Examples:

- MySQL
- PostgreSQL
- Aurora
- MariaDB
- Oracle
- SQL Server

Hands-on used:

```text
MySQL
```

---

# Templates

| Template | Purpose |
|---|---|
| Production | Multi-AZ / HA |
| Dev/Test | Testing |
| Free Tier | Cheap single instance |

---

# Multi-AZ

Production templates support:

- Multi-AZ DB instance
- Multi-AZ DB cluster

Free tier supports:

```text
Single-AZ only
```

---

# Credentials

Options:

- self-managed password
- AWS Secrets Manager

Secrets Manager is more secure but costs extra.

---

# Instance Type

Example:

```text
db.t4g.micro
```

---

# Storage

Example:

```text
20 GB
```

Can enable:

```text
Storage Auto Scaling
```

---

# Connectivity

## Public Access

```text
Public Access = Yes
```

Needed for external DB connection.

---

# Security Group

Allow inbound traffic on:

```text
Port 3306 (MySQL)
```

Usually from:

```text
Your IP address
```

---

# MySQL Default Port

```text
3306
```

---

# Connecting to DB

Used SQL Electron client.

Connection requires:

- endpoint
- port
- username
- password
- database name

---

# Endpoint

RDS provides:

```text
Database Endpoint
```

Application connects using this endpoint.

---

# Example SQL

## Create Table

```sql
CREATE TABLE mytable (
  name VARCHAR(20),
  first_name VARCHAR(20)
);
```

---

# Insert Data

```sql
INSERT INTO mytable
VALUES ('marrek', 'Stephane');
```

---

# Read Replica

Can create:

```text
Read Replica
```

Purpose:

✅ scale read traffic

---

# Monitoring

RDS exposes metrics like:

- CPU Utilization
- DB Connections
- Storage

via monitoring dashboards.

---

# Snapshots

RDS supports:

- manual snapshots
- point-in-time restore
- cross-region restore

---

# Important Features Recap

| Feature | Purpose |
|---|---|
| Read Replica | Read scaling |
| Multi-AZ | High availability |
| Snapshots | Backup/restore |
| Storage Auto Scaling | Auto increase storage |
| Monitoring | DB metrics |

---

# Important Exam Points

## RDS is Fully Managed

AWS handles:

- patching
- backups
- scaling
- monitoring

---

## Cannot SSH into RDS

❌ No server access

---

## Public Access + Security Group

Required for external DB connectivity.

---

# Cleanup

Before deleting DB:

1. Disable deletion protection
2. Delete database

Optional:

```text
Skip final snapshot
```

---

# Mental Model

```text
Application
   ↓
RDS Endpoint
   ↓
Managed Database
```

---

# One-Line Revision

> RDS lets you quickly create and manage fully managed relational databases with backups, monitoring, scaling, and high availability features.

# Amazon Aurora — Key Points


# What is Aurora?

Aurora is AWS’s proprietary relational database.

Compatible with:

- MySQL
- PostgreSQL

Applications use normal MySQL/Postgres drivers.

---

# Main Advantages

| Feature | Benefit |
|---|---|
| Cloud Optimized | Much faster than standard RDS |
| Auto Storage Scaling | 10 GB → 256 TB automatically |
| High Availability | Built-in Multi-AZ |
| Fast Failover | < 30 seconds |
| Read Scaling | Up to 15 replicas |

---

# Performance

Aurora provides approximately:

- 5x MySQL RDS performance
- 3x PostgreSQL RDS performance

---

# Storage Architecture (Very Important)

Aurora automatically stores:

```text
6 copies of data across 3 AZs
```

---

# Availability Rules

| Operation | Copies Needed |
|---|---|
| Writes | 4 of 6 |
| Reads | 3 of 6 |

Very fault tolerant.

---

# Aurora Storage Features

✅ Auto-expanding  
✅ Self-healing  
✅ Replicated across AZs

---

# Aurora Cluster Architecture

## One Writer (Master)

Handles:

```text
Writes
```

---

## Multiple Read Replicas

Handle:

```text
Reads
```

Supports:

```text
Up to 15 replicas
```

---

# Failover

If writer fails:

```text
Read replica becomes new writer
```

Failover usually:

```text
< 30 seconds
```

---

# Important Endpoints

## Writer Endpoint

Always points to:

```text
Current master/writer
```

Used for:

```text
INSERT / UPDATE / DELETE
```

---

# Reader Endpoint

Automatically load balances across replicas.

Used for:

```text
SELECT queries
```

---

# Important Exam Point

## Reader endpoint does:

✅ connection-level load balancing

NOT statement-level balancing.

---

# Read Replica Features

✅ Fast replication  
✅ Cross-region replication  
✅ Auto-scaling supported

---

# Aurora vs RDS

| Feature | RDS | Aurora |
|---|---|---|
| Storage Scaling | Manual/limited auto | Fully auto |
| Replication Speed | Slower | Faster |
| Failover | Slower | Faster |
| HA | Optional | Built-in |
| Storage Copies | Standard | 6 copies across 3 AZs |

---

# Aurora Extra Features

- Automated backups
- Point-in-time recovery
- Backtrack feature
- Zero-downtime patching
- Monitoring
- Maintenance

---

# Backtrack

Allows restoring DB to earlier point in time without restoring backups.

Example:

```text
Restore to yesterday 4 PM
```

---

# Important Exam Keywords

| Keyword | Think Aurora |
|---|---|
| Writer Endpoint | Aurora |
| Reader Endpoint | Aurora |
| Auto-expanding storage | Aurora |
| 6 copies across 3 AZs | Aurora |
| Fast failover | Aurora |

---

# Mental Model

```text
One Writer
   ↓
Shared Distributed Storage
   ↓
Multiple Read Replicas
```

---

# One-Line Revision

> Aurora is AWS’s high-performance cloud-native relational database with automatic scaling, built-in high availability, and fast read replication.

- ![alt text](image-130.png)
- ![alt text](image-131.png)
- ![alt text](image-132.png)
- ![alt text](image-133.png)


# Amazon Aurora Hands-On — Key Points


# Creating Aurora Database

## Steps

```text
RDS Console
→ Create Database
→ Aurora
```

Aurora options:

- MySQL-compatible
- PostgreSQL-compatible

---

# Templates

Used:

```text
Production
```

Allows advanced configuration.

---

# Storage Options

| Option | Purpose |
|---|---|
| Aurora Standard | Cost-effective |
| Aurora IO Optimized | Heavy read/write workloads |

---

# Instance Configuration

Example:

```text
db.t3.medium
```

---

# Aurora Serverless v2

Alternative option:

```text
Serverless v2
```

Uses:

```text
Aurora Capacity Units (ACU)
```

instead of fixed instance sizes.

---

# Aurora Replicas

Can create:

```text
Reader replicas in other AZs
```

Benefits:

- high availability
- read scaling
- faster failover

---

# Connectivity

Used:

```text
Public Access = Yes
```

for external DB access.

---

# Security Group

Allow inbound traffic on:

```text
Port 3306
```

(MySQL port)

---

# Important Aurora Endpoints

## Writer Endpoint

Used for:

```text
INSERT / UPDATE / DELETE
```

Always points to current writer instance.

---

# Reader Endpoint

Used for:

```text
SELECT queries
```

Automatically load balances across replicas.

---

# Dedicated Instance Endpoints

Each Aurora instance also has its own endpoint.

---

# Replica Auto Scaling

Aurora can auto-scale read replicas.

Example policy:

```text
Target CPU = 60%
```

If exceeded:

```text
Create more read replicas
```

---

# Replica Limits

```text
1 → 15 Aurora replicas
```

---

# Cross-Region Replicas

Aurora supports:

```text
Cross-region read replicas
```

Useful for:

- global apps
- disaster recovery

---

# Aurora Global Database

Can replicate Aurora across AWS regions.

Requires compatible Aurora version + instance type.

---

# Monitoring

Aurora supports metrics like:

- CPU utilization
- connection count
- replica metrics

---

# Backup Features

Supports:

- automated backups
- point-in-time restore
- backtracking

---

# Security Features

Supports:

- IAM authentication
- Kerberos authentication
- encryption
- deletion protection

---

# Important Exam Points

| Feature | Key Point |
|---|---|
| Writer Endpoint | Always points to writer |
| Reader Endpoint | Load balances reads |
| Aurora Replicas | Read scaling + HA |
| Serverless v2 | Auto-scaling DB compute |
| Global Database | Cross-region Aurora |

---

# Cleanup Process

To delete Aurora cluster:

1. Delete reader instances
2. Delete writer instance
3. Delete cluster

---

# Mental Model

```text
Applications
   ↓
Writer Endpoint / Reader Endpoint
   ↓
Aurora Cluster
   ↓
Shared Distributed Storage
```

---

# One-Line Revision

> Aurora provides high-performance cloud-native databases with automatic read scaling, fast failover, and global replication capabilities.


# RDS & Aurora Security — Key Points

# 1. At-Rest Encryption

Encrypts database storage/volumes.

Uses:

```text
AWS KMS(AWS Key Management System similar to Azure Key Vault for storing encryption keys)
```

Applies to:

- primary DB
- read replicas
- snapshots

---

# Important

Encryption must usually be enabled:

```text
At DB creation time
```

---

# Encrypt Existing Unencrypted DB

Steps:

```text
Take Snapshot
   ↓
Restore Snapshot as Encrypted DB
```

---

# Important Exam Point

If primary DB is NOT encrypted:

```text
Read replicas cannot be encrypted
```

---

# 2. In-Flight Encryption

Encrypts traffic between:

```text
Client ↔ Database
```

Uses:

```text
TLS/SSL
```

Clients use AWS TLS root certificates.

---

# 3. Authentication Options

## Traditional Authentication

```text
Username + Password
```

---

# IAM Authentication

Can use:

```text
IAM Roles
```

instead of DB passwords.

Very useful for EC2/ECS applications.

---

# Example

```text
EC2 IAM Role
   ↓
Authenticate to RDS
```

No hardcoded DB password needed.

---

# 4. Network Security

Uses:

```text
Security Groups
```

Control:

- ports
- IPs
- allowed services

Example:

```text
Allow MySQL Port 3306
```

---

# 5. SSH Access

❌ No SSH access

Because RDS/Aurora are managed services.

(Exception: RDS Custom)

---

# 6. Audit Logs

Can log:

- queries
- DB activity
- access events

---

# Long-Term Log Storage

Send audit logs to:

```text
CloudWatch Logs
```

---

# Important Exam Points

| Feature | Key Point |
|---|---|
| At-Rest Encryption | Uses KMS |
| In-Flight Encryption | Uses TLS |
| IAM DB Auth | Passwordless AWS auth |
| Security Groups | Network access control |
| SSH Access | Not allowed |
| Audit Logs | Can send to CloudWatch Logs |

---

# Mental Model

```text
Encryption at Rest
= Secure stored data

Encryption in Transit
= Secure network traffic
```

---

# One-Line Revision

> RDS and Aurora support encryption, IAM authentication, security groups, and audit logging for secure managed databases.


# Amazon RDS Proxy — Key Points

# What is RDS Proxy?

RDS Proxy is a fully managed database proxy for:

- RDS
- Aurora

Deployed inside your VPC.

---

# Main Purpose

RDS Proxy:

✅ pools and shares DB connections

instead of every app connecting directly to the DB.

---

# Without RDS Proxy

```text
1000 App Connections
        ↓
RDS Database
```

Too many open DB connections.

---

# With RDS Proxy

```text
Applications
      ↓
RDS Proxy
      ↓
Few Optimized DB Connections
      ↓
RDS Database
```

---

# Benefits

| Benefit | Purpose |
|---|---|
| Connection Pooling | Reduce DB load |
| Fewer Connections | Lower CPU/RAM usage |
| Reduced Timeouts | Better stability |
| Faster Failover | Better HA |
| IAM Authentication | Secure DB access |

---

# Failover Improvement

RDS Proxy can reduce failover times by:

```text
Up to 66%
```

Especially useful for:

- RDS Multi-AZ
- Aurora failover

---

# Supported Databases

- MySQL
- PostgreSQL
- MariaDB
- SQL Server
- Aurora MySQL
- Aurora PostgreSQL

---

# Application Changes

❌ No code changes needed

Just replace:

```text
DB Endpoint
```

with:

```text
RDS Proxy Endpoint
```

---

# IAM Authentication

RDS Proxy supports:

```text
IAM-based DB authentication
```

instead of username/password.

---

# Secrets Manager Integration

Credentials can be securely stored in:

```text
AWS Secrets Manager
```

---

# Important Security Point

❌ RDS Proxy is NOT publicly accessible

Accessible only inside:

```text
VPC
```

---

# Very Important Use Case — Lambda

Lambda can create massive short-lived DB connections.

Problem:

```text
1000 Lambda functions
→ 1000 DB connections
```

Can overload RDS.

---

# Solution

```text
Lambda
   ↓
RDS Proxy
   ↓
Pooled DB Connections
   ↓
RDS
```

Very common AWS architecture.

---

# Important Exam Points

| Keyword | Think RDS Proxy |
|---|---|
| Connection Pooling | RDS Proxy |
| Reduce DB connections | RDS Proxy |
| Lambda + RDS | Use RDS Proxy |
| IAM DB Authentication | RDS Proxy |
| Faster DB Failover | RDS Proxy |

---

# Mental Model

> RDS Proxy sits between apps and the database to efficiently manage DB connections.

---

# One-Line Revision

> RDS Proxy improves database scalability and reliability by pooling connections, reducing failover time, and supporting IAM authentication.

- ![alt text](image-136.png)

# Amazon ElastiCache — Key Points


# What is ElastiCache?

Managed caching service for:

- Redis
- Memcached

---

# What is a Cache?

Cache = in-memory database with:

✅ very low latency  
✅ very high performance

Used to reduce database load.

---

# Main Purpose

Instead of querying DB every time:

```text
Application
   ↓
Cache
   ↓
Database (only if needed)
```

---

# Cache Hit vs Cache Miss

## Cache Hit

```text
Data found in cache
→ Return immediately
```

Fast.

---

# Cache Miss

```text
Data not in cache
→ Query DB
→ Store result in cache
```

---

# Main Benefits

| Benefit | Purpose |
|---|---|
| Faster Reads | Low latency |
| Reduce DB Load | Fewer DB queries |
| Better Scalability | Handle more traffic |
| Stateless Apps | Store session data externally |

---

# Stateless Application Example

Store user session in ElastiCache.

```text
User Login
   ↓
Session stored in Redis
```

Any app instance can retrieve session later.

---

# Redis vs Memcached

| Feature | Redis | Memcached |
|---|---|
| High Availability | Yes | No |
| Replication | Yes | No |
| Multi-AZ | Yes | No |
| Persistence | Yes | No |
| Backup/Restore | Yes | Limited |
| Read Replicas | Yes | No |
| Advanced Data Types | Yes | No |
| Multi-threaded | No (mostly single-threaded) | Yes |

---

# Redis

Best for:

- sessions
- leaderboards
- persistence
- HA
- read replicas

Supports:

```text
Replication + Auto Failover
```

---

# Memcached

Best for:

- simple caching
- very high performance
- distributed/sharded cache

Uses:

```text
Sharding
```

No replication/high availability.

---

# Important Exam Points

| Keyword | Think |
|---|---|
| In-memory cache | ElastiCache |
| Sessions | Redis |
| Leaderboards | Redis Sorted Sets |
| Cache Hit/Miss | ElastiCache |
| Reduce DB load | ElastiCache |
| Stateless App | ElastiCache |

---

# Important Architecture

```text
Application
   ↓
ElastiCache
   ↓
RDS Database
```

---

# Important Limitation

Using cache requires:

❌ application code changes

App must explicitly:

- read/write cache
- handle invalidation

---

# Cache Invalidation

Must ensure stale data is removed/updated.

Very important caching concept.

---

# Mental Model

```text
Frequently accessed data
→ Store in RAM
→ Avoid repeated DB queries
```

---

# One-Line Revision

> ElastiCache is AWS’s managed Redis/Memcached service used to speed up applications by caching frequently accessed data in memory.


- ![alt text](image-137.png)

- ![alt text](image-138.png)

# Amazon ElastiCache Hands-On — Key Points


# Supported Engines

- Valkey
- Redis OSS
- Memcached

Hands-on used:

```text
Redis
```

---

# Deployment Options

| Option | Purpose |
|---|---|
| Serverless | AWS manages scaling |
| Node-Based Cluster | Manual cluster setup |

Hands-on used:

```text
Node-Based Cluster
```

---

# Cluster Mode

## Disabled

```text
1 primary node
+ optional read replicas
```

Simple setup.

---

# Cluster Mode Enabled

Supports:

```text
Multiple shards
```

Used for horizontal scaling.

---

# Multi-AZ

Provides:

✅ high availability  
✅ failover support

---

# Auto Failover

If primary node fails:

```text
Replica promoted automatically
```

---

# Node Type

Example:

```text
cache.t2.micro
cache.t3.micro
```

---

# Replicas

Redis supports:

```text
Read replicas
```

Used for:

- scaling reads
- high availability

---

# Subnet Group

Defines:

```text
Which subnets ElastiCache can run in
```

---

# Security Features

## Encryption At Rest

Uses encryption keys for stored cache data.

---

# Encryption In Transit

Encrypts:

```text
Client ↔ Cache traffic
```

Enables access control options.

---

# Redis Authentication

Supports:

- AUTH token (password)
- User groups / ACLs

---

# Security Groups

Control network access to cache cluster.

---

# Backup Features

Can enable:

```text
Redis backups
```

---

# Logging

Supports logs like:

- slow logs
- engine logs

Can export to:

```text
CloudWatch Logs
```

---

# Endpoints

## Primary Endpoint

Used for:

```text
Writes
```

---

# Reader Endpoint

Used for:

```text
Read replicas
```

---

# Similarity to RDS

ElastiCache console/features resemble:

```text
RDS
```

but for Redis/Memcached.

---

# Important Exam Points

| Feature | Key Point |
|---|---|
| Cluster Mode Disabled | Single shard |
| Cluster Mode Enabled | Multiple shards |
| Multi-AZ | HA + failover |
| Auto Failover | Replica promotion |
| Reader Endpoint | Read scaling |
| Security Groups | Network access control |

---

# Cleanup

Delete cluster after use to avoid charges.

---

# Mental Model

```text
Application
   ↓
ElastiCache Redis
   ↓
Fast in-memory reads
```

---

# One-Line Revision

> ElastiCache Redis provides high-speed in-memory caching with replication, failover, sharding, and security features similar to RDS.

# ElastiCache Caching Strategies — Key Points

Source: :contentReference[oaicite:0]{index=0}

# Why Use Caching?

Purpose:

✅ reduce DB load  
✅ improve read performance  
✅ lower latency

---

# Important Considerations

## Good Candidates for Caching

✅ slowly changing data  
✅ frequently accessed data  
✅ key-value lookups

Examples:

- user profiles
- blogs
- activity feeds

---

# Bad Candidates for Caching

❌ rapidly changing data  
❌ highly sensitive real-time data

Examples:

- pricing data
- bank balances

---

# 1. Lazy Loading (Cache-Aside / Lazy Population)

Most important caching strategy.

---

# Flow

```text
Application
   ↓
Check Cache

Cache Hit?
   ├── Yes → Return data
   └── No
          ↓
       Query RDS
          ↓
     Store in Cache
          ↓
      Return data
```

---

# Benefits

✅ only requested data gets cached  
✅ simple to implement  
✅ very common strategy

---

# Drawbacks

❌ first request slower (cache miss)  
❌ stale data possible  
❌ extra network calls on cache miss

---

# Cache Miss Cost

```text
Cache
→ DB
→ Cache Write
```

3 operations.

---

# Important Exam Keyword

| Keyword | Meaning |
|---|---|
| Cache-Aside | Lazy Loading |
| Lazy Population | Lazy Loading |

---

# C# Pseudocode — Lazy Loading

```csharp
public User GetUser(int userId)
{
    var user = cache.Get<User>(userId.ToString());

    if (user == null)
    {
        user = database.GetUser(userId);

        cache.Set(userId.ToString(), user);
    }

    return user;
}
```

---

# 2. Write-Through Strategy

Cache updated whenever DB updated.

---

# Flow

```text
Application
   ↓
Write to DB
   ↓
Write to Cache
```

---

# Benefits

✅ cache always fresh  
✅ avoids stale data

---

# Drawbacks

❌ slower writes  
❌ cache churn possible

(Cache stores unused data)

---

# Write Penalty

Each write requires:

```text
DB write + Cache write
```

---

# C# Pseudocode — Write Through

```csharp
public void SaveUser(User user)
{
    database.SaveUser(user);

    cache.Set(user.Id.ToString(), user);
}
```

---

# Best Practice

Often combine:

```text
Lazy Loading + Write Through
```

---

# TTL (Time To Live)

Defines how long cache item survives.

Example:

```text
TTL = 5 minutes
```

After TTL expires:

```text
Cache entry removed
```

---

# C# Example with TTL

```csharp
cache.Set(
    user.Id.ToString(),
    user,
    TimeSpan.FromMinutes(5)
);
```

---

# Cache Eviction

Cache removes items when:

- memory full
- TTL expired
- least recently used (LRU)

---

# LRU

```text
Least Recently Used
```

Old unused cache entries removed first.

---

# Important Scaling Point

Too many evictions:

```text
Increase cache size
```

(scale up or scale out)

---

# Important Exam Points

| Strategy | Purpose |
|---|---|
| Lazy Loading | Read optimization |
| Write Through | Fresh cache |
| TTL | Automatic cache cleanup |
| LRU | Eviction policy |

---

# Mental Model

```text
Frequently accessed data
→ Keep in memory
→ Avoid repeated DB calls
```

---

# One-Line Revision

> Lazy Loading improves read performance, Write-Through keeps cache fresh, and TTL/LRU manage cache expiration and eviction.


- ![alt text](image-140.png)
- ![alt text](image-141.png)
- ![alt text](image-142.png)
- ![alt text](image-143.png)

# Amazon MemoryDB for Redis — Key Points

# What is MemoryDB?

MemoryDB is:

```text
Redis-compatible in-memory database
```

Unlike ElastiCache Redis:

```text
MemoryDB is intended to be a primary database
```

not just a cache.

---

# Main Difference

| ElastiCache Redis | MemoryDB |
|---|---|
| Primarily cache | Primary durable DB |
| Some durability | Strong durability |
| Cache workloads | Database workloads |

---

# Key Features

| Feature | Purpose |
|---|---|
| Redis-Compatible API | Use Redis clients/tools |
| In-Memory Performance | Ultra low latency |
| Durable Storage | Persistent data |
| Multi-AZ Transaction Log | High availability |
| Fast Recovery | Durable failover |

---

# Performance

Supports:

```text
160+ million requests/sec
```

Very high throughput.

---

# Durability

Data stored using:

```text
Multi-AZ transaction log
```

Provides:

✅ durability  
✅ recovery  
✅ failover support

---

# Scalability

Scales from:

```text
Tens of GBs
→ Hundreds of TBs
```

---

# Common Use Cases

- microservices
- gaming
- mobile apps
- streaming
- real-time apps

---

# Important Exam Point

## MemoryDB vs Redis Cache

### ElastiCache Redis

```text
Fast cache
```

### MemoryDB

```text
Fast durable database
```

---

# Example Architecture

```text
Microservices
      ↓
MemoryDB for Redis
      ↓
Durable In-Memory Data
```

---

# Mental Model

```text
Redis Speed
+ Database Durability
```

---

# One-Line Revision

> MemoryDB is a durable Redis-compatible in-memory database designed for ultra-fast applications requiring persistence and high availability.


- ![alt text](image-144.png)


# Route 53

# DNS Basics — Key Points


# What is DNS?

DNS = Domain Name System

Purpose:

```text
Convert domain names → IP addresses
```

Example:

```text
google.com → 142.x.x.x
```

---

# Why DNS Exists

Humans remember:

```text
example.com
```

Computers communicate using:

```text
IP addresses
```

DNS translates between them.

---

# DNS Hierarchy

Example:

```text
api.www.example.com
```

| Part | Meaning |
|---|---|
| .com | Top Level Domain (TLD) |
| example.com | Second-Level Domain |
| www.example.com | Subdomain |
| api.www.example.com | FQDN |

---

# Important Terms

| Term | Meaning |
|---|---|
| Domain Registrar | Where domains are bought |
| DNS Record | Maps domain → target |
| Name Server (NS) | Resolves DNS queries |
| Zone File | Contains DNS records |
| TLD | .com, .org, .in |

---

# Common Domain Registrars

- Route 53
- GoDaddy

---

# DNS Record Types

Examples:

- A
- AAAA
- CNAME
- NS

(covered later in Route 53)

---

# Important DNS Concept

## A Record

Maps:

```text
Domain → IPv4 Address
```

Example:

```text
example.com → 9.10.11.12
```

---

# How DNS Resolution Works

## Step 1

Browser asks:

```text
What is example.com?
```

to:

```text
Local DNS Resolver
```

(usually ISP/company DNS)

---

# Step 2

Local DNS asks:

```text
Root DNS Server
```

---

# Step 3

Root server says:

```text
Ask .com TLD server
```

---

# Step 4

TLD server says:

```text
Ask example.com name server
```

---

# Step 5

Authoritative DNS server returns:

```text
example.com → 9.10.11.12
```

---

# Step 6

Browser uses IP to connect to server.

---

# Important Organizations

| Organization | Purpose |
|---|---|
| ICANN | Root DNS management |
| IANA | TLD management |

---

# DNS Caching

Resolvers cache DNS results for faster future lookups.

---

# Example Full Flow

```text
Browser
   ↓
Local DNS
   ↓
Root DNS
   ↓
.com TLD Server
   ↓
Authoritative DNS Server
   ↓
IP Address Returned
```

---

# Important Exam Points

| Keyword | Meaning |
|---|---|
| DNS | Domain → IP translation |
| TLD | .com / .org |
| NS Record | Name server info |
| A Record | Domain → IPv4 |
| FQDN | Fully qualified domain name |

---

# Mental Model

```text
DNS = Internet phonebook
```

Maps human-friendly names to machine IPs.

---

# One-Line Revision

> DNS translates domain names into IP addresses using hierarchical name servers and DNS records.


- ![alt text](image-146.png)

# Amazon Route 53 — Key Points

# What is Route 53?

Route 53 is AWS’s:

✅ managed DNS service  
✅ domain registrar  
✅ authoritative DNS

---

# What Does DNS Do?

```text
Domain Name → IP Address
```

Example:

```text
example.com → 54.22.33.44
```

---

# Why “Route 53”?

Named after:

```text
DNS Port 53
```

---

# Main Features

| Feature | Purpose |
|---|---|
| DNS Management | Manage domain records |
| Domain Registration | Buy domains |
| Health Checks | Monitor endpoints |
| High Availability | 100% SLA |

---

# Route 53 Flow

```text
Client
   ↓
Route 53 DNS Query
   ↓
Returns Server IP
   ↓
Client connects to EC2
```

---

# DNS Records

Records define:

```text
How traffic routes to resources
```

---

# Record Components

| Component | Meaning |
|---|---|
| Domain Name | example.com |
| Record Type | A / AAAA / CNAME |
| Value | IP / hostname |
| TTL | Cache duration |
| Routing Policy | How traffic routed |

---

# Important Record Types

## A Record

Maps:

```text
Domain → IPv4
```

Example:

```text
example.com → 1.2.3.4
```

---

# AAAA Record

Maps:

```text
Domain → IPv6
```

---

# CNAME Record

Maps:

```text
Hostname → Another Hostname
```

Example:

```text
api.example.com
    ↓
my-alb.amazonaws.com
```

---

# Important CNAME Limitation

❌ Cannot create CNAME for root domain.

Invalid:

```text
example.com
```

Valid:

```text
www.example.com
```

---

# NS Record

NS = Name Server

Defines which DNS servers are authoritative for domain.

---

# Hosted Zones

Hosted Zone = container for DNS records.

---

# Types of Hosted Zones

| Type | Purpose |
|---|---|
| Public Hosted Zone | Internet-accessible DNS |
| Private Hosted Zone | Internal VPC DNS |

---

# Public Hosted Zone

Accessible from internet.

Example:

```text
app.example.com
```

---

# Private Hosted Zone

Accessible only inside VPC.

Example:

```text
database.company.internal
```

Used for:

- internal services
- private APIs
- databases

---

# Private Hosted Zone Flow

```text
EC2
   ↓
api.company.internal
   ↓
Private IP returned
```

---

# Important Pricing

| Item | Cost |
|---|---|
| Hosted Zone | ~$0.50/month |
| Domain Registration | ~$12/year |

---

# Important Exam Points

| Keyword | Think |
|---|---|
| A Record | IPv4 mapping |
| AAAA | IPv6 mapping |
| CNAME | Hostname → hostname |
| NS | Name servers |
| Public Hosted Zone | Internet DNS |
| Private Hosted Zone | VPC-only DNS |

---

# Mental Model

```text
Route 53 = AWS DNS manager
```

Controls how domain names resolve to resources.

---

# One-Line Revision

> Route 53 is AWS’s managed DNS service used to route domain names to AWS or external resources using DNS records and hosted zones.


- ![alt text](image-148.png)

- ![alt text](image-149.png)


# Route 53 Domain Registration — Key Points

Source: 

# Registering a Domain in Route 53

## Steps

```text
Route 53
→ Register Domains
→ Enter domain name
→ Checkout
```

Example:

```text
mydomain.com
```

---

# Important Cost

Typical domain registration:

```text
~$12–13/year
```

---

# Auto Renew

| Option | Meaning |
|---|---|
| Enabled | Domain renews automatically |
| Disabled | Domain expires after period |

---

# Privacy Protection

Recommended:

✅ enable privacy protection

Hides:

- address
- phone number
- email

from public WHOIS records.

---

# After Registration

AWS automatically creates:

```text
Hosted Zone
```

for your domain.

---

# Default Records Created

## 1. NS Record

Name Server record.

Tells internet:

```text
Which DNS servers are authoritative
```

for your domain.

---

# 2. SOA Record

SOA = Start Of Authority

Contains DNS administrative information.

---

# Important Concept

Once hosted zone exists:

```text
Route 53 becomes source of truth for DNS
```

---

# Example

```text
example.com
```

DNS records added in Route 53 determine:

```text
Which IP/domain users connect to
```

---

# Typical Next Step

Add records like:

- A Record
- CNAME
- ALIAS

to route traffic.

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Hosted Zone | Container for DNS records |
| NS Record | Authoritative DNS servers |
| SOA Record | DNS authority metadata |
| Privacy Protection | Hide personal info |

---

# Mental Model

```text
Buy Domain
   ↓
Hosted Zone Created
   ↓
Add DNS Records
   ↓
Internet can resolve your domain
```

---

# One-Line Revision

> Registering a domain in Route 53 automatically creates a hosted zone with NS and SOA records for DNS management.

- ![alt text](image-150.png)
- ![alt text](image-151.png)
- ![alt text](image-152.png)
- ![alt text](image-153.png)


# Route 53 DNS Records Hands-On — Key Points

Source: 

# Creating a DNS Record

## Steps

```text
Route 53
→ Hosted Zone
→ Create Record
```

---

# Example Record

## Domain

```text
test.example.com
```

---

# Record Type

```text
A Record
```

Maps:

```text
Domain → IPv4 Address
```

---

# Example Value

```text
11.22.33.44
```

Meaning:

```text
test.example.com → 11.22.33.44
```

---

# TTL (Time To Live)

Example:

```text
300 seconds
```

Defines how long DNS resolvers cache result.

---

# Routing Policy

Default:

```text
Simple Routing
```

(More routing policies covered later)

---

# DNS Resolution Flow

```text
Browser / Client
      ↓
Route 53 Hosted Zone
      ↓
Returns IP Address
```

---

# Important Concept

Creating DNS record does NOT create server.

It only creates:

```text
Domain → IP mapping
```

---

# Verifying DNS Records

Use commands like:

```bash
nslookup test.example.com
```

or

```bash
dig test.example.com
```

---

# Example Output

```text
test.example.com → 11.22.33.44
```

---

# nslookup

Simple DNS lookup tool.

---

# dig

More detailed DNS debugging tool.

Shows:

- TTL
- record type
- DNS answer section

---

# CloudShell

AWS CloudShell provides Linux terminal inside AWS console.

Can install tools like:

```bash
sudo yum install -y bind-utils
```

to use:

- dig
- nslookup

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| A Record | Domain → IPv4 |
| TTL | DNS cache duration |
| nslookup | DNS lookup tool |
| dig | Advanced DNS query tool |

---

# Mental Model

```text
DNS Record
   ↓
Maps domain name
   ↓
To server IP
```

---

# One-Line Revision

> Route 53 DNS records map domain names to IP addresses and can be verified using tools like nslookup or dig.

- ![alt text](image-154.png)
- ![alt text](image-155.png)
- ![alt text](image-156.png)


# Now we have created the A record in Route 53, which points a domain address to an IP Address. Now we want to link it to real EC2 server, so that real data is served.

# Route 53 Setup for Routing Demo — Key Points


# Goal of Setup

Create:

- multiple EC2 instances in different AWS regions
- one ALB
- later use Route 53 routing policies

---

# Infrastructure Created

| Resource | Region |
|---|---|
| EC2 Instance | Frankfurt |
| EC2 Instance | N. Virginia |
| EC2 Instance | Singapore |
| ALB | Frankfurt |

---

# EC2 Setup

## Instance Type

```text
t2.micro
```

---

# Security Group

Allowed:

- SSH
- HTTP

from:

```text
0.0.0.0/0
```

---

# User Data Script

Bootstrap script used to display:

```text
Hello World + Availability Zone
```

---

# Example Output

```text
Hello from eu-central-1b
```

Useful for identifying which server handled request.

---

# Important AWS Concept

## User Data

Script executed automatically during EC2 startup.

Commonly used for:

- app installation
- nginx/apache setup
- bootstrap config

---

# ALB Setup

## Type

```text
Application Load Balancer
```

---

# Listener

```text
HTTP : 80
```

---

# Target Group

Configured with:

```text
EC2 instances
```

---

# ALB Flow

```text
Client
   ↓
ALB
   ↓
EC2 Target Group
```

---

# Important Step

Registered Frankfurt EC2 instance into target group.

---

# ALB DNS Name

AWS automatically generates:

```text
my-alb.amazonaws.com
```

Used later in Route 53.

---

# Validation Performed

Verified:

✅ EC2 public IPs working  
✅ ALB endpoint working

---

# Key AWS Concepts Used

| Concept | Purpose |
|---|---|
| EC2 User Data | Bootstrap setup |
| Security Group | Network access |
| ALB | HTTP load balancing |
| Target Group | Backend instances |
| ALB DNS Name | Public load balancer endpoint |

---

# Important Exam Points

| Keyword | Think |
|---|---|
| User Data | Startup script |
| ALB | Layer 7 load balancer |
| Target Group | Backend servers |
| DNS Name | Route traffic |

---

# Mental Model

```text
Users
   ↓
Route 53
   ↓
ALB / EC2
   ↓
Application Response
```

---

# One-Line Revision

> Multiple EC2 instances and an ALB were created across regions to later demonstrate Route 53 DNS routing policies.


# Route 53 TTL (Time To Live) — Key Points


# What is TTL?

TTL = Time To Live

Defines:

```text
How long DNS responses are cached
```

by clients/resolvers.

---

# Example

```text
TTL = 300 seconds
```

Meaning:

```text
Client caches DNS result for 5 minutes
```

---

# DNS Flow with TTL

```text
Client
   ↓
Route 53 DNS Query
   ↓
Returns IP + TTL
   ↓
Client caches result
```

---

# During TTL Period

Client:

❌ does NOT query DNS again

Uses cached IP directly.

---

# Benefit of TTL

Reduces:

✅ DNS traffic  
✅ Route 53 queries  
✅ latency

---

# High TTL vs Low TTL

| TTL Type | Pros | Cons |
|---|---|---|
| High TTL (24h) | Fewer DNS queries | Slow record updates |
| Low TTL (60s) | Faster updates | More DNS traffic/cost |

---

# High TTL Example

```text
TTL = 24 hours
```

If record changes:

```text
Clients may still use old IP for 24h
```

---

# Low TTL Example

```text
TTL = 60 seconds
```

Changes propagate much faster.

---

# Common Strategy

Before changing DNS record:

1. Lower TTL
2. Wait for cache refresh
3. Change record
4. Increase TTL later

---

# Important Exam Point

TTL required for:

✅ most Route 53 records

Exception:

```text
Alias Records
```

---

# Hands-On Example

Created:

```text
demo.example.com
```

with:

```text
TTL = 120 seconds
```

---

# dig Command Observation

Initial output:

```text
TTL = 115
```

Later:

```text
TTL = 98
```

TTL countdown decreases as cache ages.

---

# Important Concept

Changing Route 53 record does NOT instantly update users.

Users continue using cached value until:

```text
TTL expires
```

---

# Verification Tools

## nslookup

```bash
nslookup demo.example.com
```

---

# dig

```bash
dig demo.example.com
```

Shows:

- IP
- record type
- TTL remaining

---

# Example Scenario

## Before TTL Expiry

```text
Browser still connects to old server
```

even after DNS record changed.

---

# After TTL Expiry

```text
Browser queries Route 53 again
→ gets new IP
```

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| TTL | DNS cache duration |
| High TTL | Less DNS traffic |
| Low TTL | Faster DNS changes |
| dig | Shows TTL countdown |

---

# Mental Model

```text
TTL = Expiration timer for DNS cache
```

---

# One-Line Revision

> TTL controls how long DNS responses are cached before clients query Route 53 again.

- ![alt text](image-157.png)
- ![alt text](image-158.png)

# Route 53 CNAME vs Alias Records — Key Points


# Why Needed?

AWS resources like:

- ALB
- CloudFront
- API Gateway

expose DNS names like:

```text
my-alb.amazonaws.com
```

You usually want:

```text
app.mydomain.com
```

---

# 1. CNAME Record

Maps:

```text
Hostname → Another Hostname
```

Example:

```text
app.mydomain.com
    ↓
my-alb.amazonaws.com
```

---

# Important Limitation

❌ Cannot use CNAME at root domain (Zone Apex)

Invalid:

```text
mydomain.com
```

Valid:

```text
app.mydomain.com
```

---

# 2. Alias Record (AWS Specific)

AWS Route 53 feature.

Maps:

```text
Domain → AWS Resource
```

Example:

```text
mydomain.com
    ↓
ALB
```

---

# Major Alias Benefits

| Benefit | Meaning |
|---|---|
| Works at Root Domain | Supports apex |
| Free Queries | No Route 53 charge |
| Health Check Integration | Native AWS support |
| Auto IP Updates | Tracks AWS resource IP changes |

---

# Important Alias Features

## Alias supports:

✅ root domains  
✅ subdomains

---

# Alias Automatically Tracks

If ALB IP changes:

```text
Alias updates automatically
```

---

# Alias Record Types

Always:

```text
A or AAAA
```

---

# TTL Behavior

## CNAME

```text
You configure TTL
```

---

# Alias

```text
TTL managed automatically by Route 53
```

---

# Supported Alias Targets

- ALB / ELB
- CloudFront
- API Gateway
- S3 Static Website
- Elastic Beanstalk
- Global Accelerator
- Route 53 records

---

# Important Limitation

❌ Cannot create Alias to EC2 public DNS name

---

# Hands-On Example

## CNAME

```text
myapp.example.com
    ↓
my-alb.amazonaws.com
```

Worked successfully.

---

# Root Domain Attempt with CNAME

```text
example.com
```

❌ Failed

Error:

```text
CNAME not permitted at apex of zone
```

---

# Solution

Use:

```text
Alias A Record
```

instead.

---

# Important Exam Points

| Feature | CNAME | Alias |
|---|---|---|
| Standard DNS | Yes | AWS-specific |
| Root Domain Support | No | Yes |
| Free Queries | No | Yes |
| AWS Resource Integration | Limited | Native |
| Auto IP Tracking | No | Yes |

---

# Mental Model

## CNAME

```text
Hostname → Hostname
```

## Alias

```text
Route 53 shortcut → AWS resource
```

---

# One-Line Revision

> Use Alias records for AWS resources and root domains, while CNAME records are used for subdomains pointing to other hostnames.

- ![alt text](image-159.png)
- ![alt text](image-160.png)


# Route 53 Simple Routing Policy — Key Points

Source: 

# What is Simple Routing?

Default Route 53 routing policy.

Used to:

```text
Route traffic to single resource
```

---

# Example

```text
simple.example.com
    ↓
1.2.3.4
```

---

# Important Concept

DNS does NOT route traffic itself.

Route 53 only:

```text
Returns DNS answers
```

Client then connects directly to returned IP.

---

# Supported Routing Policies

- Simple
- Weighted
- Failover
- Latency-based
- Geolocation
- Multi-value
- Geoproximity

---

# Simple Routing Behavior

## Single Value

```text
One domain
→ One IP
```

---

# Multiple Values

Simple routing can return:

```text
Multiple IP addresses
```

Example:

```text
simple.example.com
→ 1.1.1.1
→ 2.2.2.2
```

---

# Client Behavior

Client/browser randomly chooses one returned IP.

---

# Important Limitation

❌ No health checks supported

with simple routing.

---

# Alias Record Limitation

If using Alias + Simple Routing:

```text
Only one AWS resource allowed
```

---

# Hands-On Example

Created:

```text
simple.example.com
```

with:

```text
TTL = 20 seconds
```

---

# Multiple IP Demo

Added:

- Singapore EC2
- US-East EC2

Both returned in DNS response.

---

# dig Output

```text
Answer Section:
→ IP 1
→ IP 2
```

---

# Result

Refreshing browser after TTL expiry randomly hit:

- Singapore EC2
- US-East EC2

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Simple Routing | Basic DNS routing |
| Multiple IPs | Client randomly selects |
| Health Checks | Not supported |
| TTL | DNS cache duration |

---

# Mental Model

```text
Route 53
   ↓
Returns one or more IPs
   ↓
Client chooses destination
```

---

# One-Line Revision

> Simple routing returns one or more DNS records, and clients randomly choose the destination when multiple values are returned.

- ![alt text](image-161.png)
- ![alt text](image-162.png)
- ![alt text](image-163.png)


# Route 53 Weighted Routing Policy — Key Points 

# What is Weighted Routing?

Lets you control:

```text
What % of DNS traffic goes to which resource
```

using weights.

---

# Example

| Server | Weight |
|---|---|
| EC2-1 | 70 |
| EC2-2 | 20 |
| EC2-3 | 10 |

Traffic distribution:

- 70% → EC2-1
- 20% → EC2-2
- 10% → EC2-3

---

# Formula

```text
Traffic % =
Record Weight / Total Weight
```

---

# Important

Weights:

❌ do NOT need to sum to 100

Only relative values matter.

---

# Requirements

All weighted records must have:

✅ same domain name  
✅ same record type

---

# Use Cases

| Use Case | Purpose |
|---|---|
| Load Balancing | Spread traffic |
| Canary Deployment | Test new app version |
| Gradual Migration | Shift traffic slowly |

---

# Weight = 0

```text
Stops sending traffic to resource
```

---

# Important Edge Case

If ALL records have:

```text
Weight = 0
```

then Route 53 returns all records equally.

---

# Health Checks

Weighted records can be associated with:

✅ health checks

(unlike simple routing)

---

# Hands-On Example

Created:

```text
weighted.example.com
```

with:

| Region | Weight |
|---|---|
| US-East | 70 |
| EU-Central | 20 |
| AP-Southeast | 10 |

---

# Result

Most DNS queries resolved to:

```text
US-East
```

because it had highest weight.

Occasionally resolved to:

- EU
- Singapore

---

# TTL Used

```text
3 seconds
```

Used only for demo purposes.

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Weighted Routing | Percentage-based routing |
| Higher Weight | More traffic |
| Weight = 0 | No traffic |
| Same Name/Type Required | Mandatory |
| Health Checks Supported | Yes |

---

# Mental Model

```text
Route 53
   ↓
Chooses DNS answer
based on weight
```

---

# One-Line Revision

> Weighted routing distributes DNS responses across resources based on assigned relative weights.

- ![alt text](image-164.png)
- ![alt text](image-165.png)

# Route 53 Latency-Based Routing — Key Points

Source: 

# What is Latency-Based Routing?

Routes users to:

```text
Region with lowest network latency
```

---

# Goal

Improve:

✅ response time  
✅ user experience

---

# Important Concept

Routing is based on:

```text
Lowest latency to AWS region
```

NOT geographical distance.

Closest region ≠ lowest latency always.

---

# Example

| User Location | Routed To |
|---|---|
| Germany | eu-central-1 |
| Canada | us-east-1 |
| Hong Kong | ap-southeast-1 |

---

# Architecture Example

```text
Users
   ↓
Route 53
   ↓
Closest/Lowest Latency Region
```

---

# Requirements

When using IP addresses directly:

✅ must specify AWS region manually

because Route 53 cannot infer region from raw IP.

---

# Health Checks

Latency records support:

✅ health checks

---

# Hands-On Example

Created records for:

- us-east-1
- eu-central-1
- ap-southeast-1

---

# Result

## From Europe

Returned:

```text
eu-central-1
```

---

# Using VPN to Canada

Returned:

```text
us-east-1
```

---

# Using VPN to Hong Kong

Returned:

```text
ap-southeast-1
```

---

# Important Observation

CloudShell still returned:

```text
eu-central-1
```

because CloudShell itself was running in Europe.

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Latency Routing | Lowest latency region |
| Region Required | Needed for raw IPs |
| Health Checks | Supported |
| VPN Testing | Simulates user location |

---

# Mental Model

```text
Route 53
   ↓
Chooses region with best latency
   ↓
Returns that endpoint IP
```

---

# One-Line Revision

> Latency-based routing directs users to the AWS region with the lowest network latency for better performance.

- ![alt text](image-166.png)
- ![alt text](image-167.png)


# Route 53 Health Checks — Key Points


# What are Route 53 Health Checks?

Used to monitor:

```text
Whether endpoints are healthy or unhealthy
```

Mainly used for:

✅ automated DNS failover

---

# Example

```text
Users
   ↓
Route 53
   ↓
Healthy Load Balancer Only
```

If one region fails:

```text
Route 53 stops returning that endpoint
```

---

# Types of Health Checks

| Type | Purpose |
|---|---|
| Endpoint Health Check | Monitor public endpoint |
| Calculated Health Check | Combine multiple health checks |
| CloudWatch Alarm Health Check | Monitor private resources |

---

# 1. Endpoint Health Check

Monitors:

- ALB
- EC2
- public APIs
- public endpoints

---

# Supported Protocols

- HTTP
- HTTPS
- TCP

---

# Health Check Behavior

AWS health checkers from around the world send requests.

If endpoint returns:

```text
2xx or 3xx
```

then endpoint is healthy.

---

# Health Check Frequency

| Type | Interval |
|---|---|
| Standard | 30 sec |
| Fast | 10 sec |

---

# Important Threshold

If more than:

```text
18% of health checkers
```

report healthy:

```text
Endpoint considered healthy
```

---

# String Matching

Health checks can validate response body text.

Checks first:

```text
5120 bytes
```

of response.

---

# Important Network Requirement

Your endpoint must allow incoming traffic from:

```text
Route 53 health checker IPs
```

---

# 2. Calculated Health Checks

Combines multiple health checks.

---

# Example

```text
HealthCheck-1
HealthCheck-2
HealthCheck-3
        ↓
Parent Health Check
```

---

# Supported Logic

- AND
- OR
- NOT

---

# Limits

Supports up to:

```text
256 child health checks
```

---

# Common Use Case

Maintenance mode without failing entire system.

---

# 3. CloudWatch Alarm Health Check

Used for:

✅ private resources  
✅ VPC/internal systems

because Route 53 cannot directly access private endpoints.

---

# Architecture

```text
Private EC2
    ↓
CloudWatch Metric
    ↓
CloudWatch Alarm
    ↓
Route 53 Health Check
```

---

# Example

Monitor:

- CPU
- memory
- custom metric

If alarm triggers:

```text
Route 53 marks endpoint unhealthy
```

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Health Checks | DNS failover |
| Public Endpoint Required | For direct health checks |
| CloudWatch Alarm Checks | For private resources |
| 2xx/3xx | Healthy responses |
| Fast Health Check | Every 10 sec |

---

# Mental Model

```text
Healthy Endpoint
→ Returned by Route 53

Unhealthy Endpoint
→ Removed from DNS answers
```

---

# One-Line Revision

> Route 53 health checks monitor endpoint availability and enable automatic DNS failover for public and private resources.

- ![alt text](image-168.png)
- ![alt text](image-170.png)
- ![alt text](image-171.png)
- ![alt text](image-172.png)

# Route 53 Health Checks Hands-On — Key Points



# Creating Health Checks

## Steps

```text
Route 53
→ Health Checks
→ Create Health Check
```

---

# Endpoint Health Check Example

Configured for:

```text
Public EC2 IP
```

using:

- IP address
- Port 80
- Path `/`

---

# Common Health Endpoint

Very common production pattern:

```text
/health
```

Example:

```text
https://api.example.com/health
```

---

# Health Check Frequency

| Type | Interval |
|---|---|
| Standard | 30 sec |
| Fast | 10 sec |

Fast checks cost more.

---

# Advanced Features

| Feature | Purpose |
|---|---|
| String Matching | Validate response text |
| Latency Graph | Monitor response time |
| Invert Health Status | Reverse healthy/unhealthy |
| Region Selection | Choose health checker regions |

---

# String Matching

Can verify response contains expected text within:

```text
First 5120 bytes
```

---

# Failure Threshold

Can configure:

```text
How many failed checks before unhealthy
```

---

# Health Check Failure Demo

Removed:

```text
HTTP Port 80 rule
```

from EC2 security group.

---

# Result

Health check became:

```text
Unhealthy
```

because Route 53 could no longer connect.

---

# Important Observation

Error showed:

```text
Connection timeout
```

due to security group blocking traffic.

---

# Important Networking Point

Route 53 health checkers must be allowed in:

```text
Security Group / Firewall
```

---

# Calculated Health Checks

Combines multiple health checks.

---

# Example

```text
3 Child Health Checks
        ↓
Calculated Parent Check
```

---

# Logic Example

Healthy only if:

```text
ALL child health checks are healthy
```

(AND logic)

---

# CloudWatch Alarm Health Checks

Used for:

✅ private EC2 instances  
✅ internal resources

---

# Flow

```text
Private Resource
    ↓
CloudWatch Metric
    ↓
CloudWatch Alarm
    ↓
Route 53 Health Check
```

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Endpoint Health Check | Monitor public endpoint |
| Calculated Health Check | Combine multiple checks |
| CloudWatch Alarm Check | Monitor private resources |
| Security Groups | Must allow health checker traffic |
| `/health` Endpoint | Common production practice |

---

# Mental Model

```text
Healthy endpoint
→ Route 53 returns DNS record

Unhealthy endpoint
→ Route 53 removes endpoint
```

---

# One-Line Revision

> Route 53 health checks monitor endpoint availability and support advanced failover logic using endpoint, calculated, and CloudWatch-based checks.


# Route 53 Failover Routing Policy — Key Points

Source: :contentReference[oaicite:0]{index=0}

# What is Failover Routing?

Used for:

✅ disaster recovery  
✅ automatic DNS failover

---

# Architecture

```text
Users
   ↓
Route 53
   ↓
Primary Endpoint
   ↓
If unhealthy
   ↓
Secondary Endpoint
```

---

# Components

| Role | Purpose |
|---|---|
| Primary Record | Main endpoint |
| Secondary Record | Backup / DR endpoint |
| Health Check | Detect failure |

---

# Important Rule

✅ Primary record MUST have health check

Secondary health check is optional.

---

# How It Works

## Normal State

If primary health check is healthy:

```text
Route 53 returns primary endpoint
```

---

# Failure State

If primary becomes unhealthy:

```text
Route 53 automatically returns secondary endpoint
```

---

# Example Setup

| Region | Role |
|---|---|
| eu-central-1 | Primary |
| us-east-1 | Secondary |

---

# Hands-On Demo

Created:

```text
failover.example.com
```

with:

- primary = EU region
- secondary = US region

---

# Failure Simulation

Removed:

```text
HTTP port 80 rule
```

from EU security group.

---

# Result

Health check became:

```text
Unhealthy
```

---

# Automatic Failover

Next DNS resolution returned:

```text
US-East endpoint
```

instead of EU.

---

# Recovery

Re-added HTTP rule to security group.

Primary became healthy again.

Traffic returned to primary.

---

# Important TTL Point

Used:

```text
TTL = 60 seconds
```

DNS failover speed depends partly on TTL.

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Failover Routing | DNS disaster recovery |
| Primary Record | Active endpoint |
| Secondary Record | Backup endpoint |
| Health Check Required | For primary |
| TTL | Affects failover speed |

---

# Mental Model

```text
Primary Healthy
→ Use Primary

Primary Unhealthy
→ Switch to Secondary
```

---

# One-Line Revision

> Failover routing automatically redirects DNS traffic from a failed primary endpoint to a healthy secondary endpoint using Route 53 health checks.

- ![alt text](image-173.png)
- ![alt text](image-174.png)


# Route 53 Geolocation Routing — Key Points


# What is Geolocation Routing?

Routes users based on:

```text
User’s physical location
```

Examples:

- continent
- country
- US state

---

# Important Difference

## Geolocation Routing

```text
Based on user location
```

---

# Latency Routing

```text
Based on lowest network latency
```

These are NOT the same.

---

# Example

| User Location | Routed To |
|---|---|
| India | Singapore |
| USA | US-East |
| Other Countries | EU Default |

---

# Common Use Cases

| Use Case | Purpose |
|---|---|
| Website Localization | Country-specific content |
| Content Restriction | Regional access control |
| Regional Load Balancing | Closest regional deployment |

---

# Important Rule

Always create:

```text
Default record
```

for unmatched locations.

---

# Example Architecture

```text
India Users
   ↓
Singapore App

US Users
   ↓
US App

Everyone Else
   ↓
EU App
```

---

# Supported Geographic Levels

- continent
- country
- US state

Most specific match wins.

---

# Health Checks

Geolocation records support:

✅ health checks

---

# Hands-On Example

Created records:

| Location Rule | Target |
|---|---|
| Asia | ap-southeast-1 |
| United States | us-east-1 |
| Default | eu-central-1 |

---

# Result

## From Europe

Returned:

```text
eu-central-1
```

(default)

---

# Using VPN to India

Returned:

```text
ap-southeast-1
```

---

# Using VPN to USA

Returned:

```text
us-east-1
```

---

# Using VPN to Mexico

Returned:

```text
eu-central-1
```

because Mexico had no explicit rule.

---

# Important Troubleshooting Point

Timeout occurred because:

```text
Security Group blocked HTTP traffic
```

Fix:

```text
Re-enable port 80 inbound rule
```

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Geolocation Routing | Based on user location |
| Default Record | Strongly recommended |
| Most Specific Match Wins | Country > continent |
| Health Checks | Supported |

---

# Mental Model

```text
User Location
   ↓
Route 53 chooses matching region
   ↓
Returns region endpoint
```

---

# One-Line Revision

> Geolocation routing directs users to region-specific endpoints based on their physical geographic location.


- ![alt text](image-175.png)


# Route 53 Geoproximity Routing — Key Points

# What is Geoproximity Routing?

Routes users based on:

```text
User geographic location
+ resource location
+ bias
```

---

# Main Purpose

Allows you to:

```text
Shift traffic toward specific regions
```

using a value called:

```text
Bias
```

---

# Important Concept

## Positive Bias

```text
More traffic to region
```

---

# Negative Bias

```text
Less traffic to region
```

---

# Without Bias

Example:

```text
US-West-1      US-East-1
     |-------------|
```

Traffic divided roughly equally based on geography.

---

# With Positive Bias

Example:

```text
US-East-1 Bias = +50
```

Result:

```text
More users routed to US-East-1
```

Boundary shifts toward west.

---

# Visual Mental Model

## No Bias

```text
West Users → US-West
East Users → US-East
```

---

# Positive Bias on East

```text
More central users now go to US-East
```

---

# Resource Types Supported

| Resource Type | Configuration |
|---|---|
| AWS Resources | Specify AWS region |
| Non-AWS / On-Prem | Specify latitude & longitude |

---

# Important Requirement

Geoproximity routing requires:

```text
Route 53 Traffic Flow
```

(advanced Route 53 feature)

---

# Main Use Case

## Traffic Shifting

Example:

```text
Increase traffic to newer region
```

without changing application code.

---

# Important Difference

| Routing Type | Based On |
|---|---|
| Geolocation | User’s actual country/location |
| Latency | Lowest network latency |
| Geoproximity | Geography + configurable bias |

---

# Important Exam Point

## Bias changes routing boundaries

Higher bias:

```text
Expands region influence
```

Lower/negative bias:

```text
Shrinks region influence
```

---

# Example Use Cases

- gradual regional migration
- traffic redistribution
- capacity balancing
- regional rollout

---

# Mental Model

```text
Bias = Magnet Strength
```

Higher bias pulls more nearby users toward region.

---

# One-Line Revision

> Geoproximity routing uses geographic distance and configurable bias values to shift DNS traffic toward specific regions.

- ![alt text](image-176.png)

# Route 53 IP-Based Routing — Key Points

# What is IP-Based Routing?

Routes users based on:

```text
Client IP address range (CIDR)
```

---

# How It Works

You define:

```text
CIDR Block → Endpoint Mapping
```

Example:

```text
203.x.x.x/24 → Server A
200.x.x.x/24 → Server B
```

---

# Important Concept

Route 53 checks:

```text
Which CIDR range client IP belongs to
```

then returns matching endpoint.

---

# Example Architecture

```text
Client IP
   ↓
Matches CIDR block
   ↓
Route 53 returns mapped endpoint
```

---

# Example

| Client CIDR | Returned Endpoint |
|---|---|
| 203.x.x.x/24 | 1.2.3.4 |
| 200.x.x.x/24 | 5.6.7.8 |

---

# Common Use Cases

| Use Case | Purpose |
|---|---|
| ISP-Based Routing | Route specific ISPs differently |
| Performance Optimization | Direct known networks optimally |
| Network Cost Reduction | Optimize traffic paths |
| Enterprise Routing | Route corporate IP ranges |

---

# Important Term

## CIDR

CIDR = IP range notation.

Example:

```text
192.168.1.0/24
```

---

# Difference from Geolocation

| Policy | Based On |
|---|---|
| Geolocation | User country/location |
| IP-Based | Exact client IP ranges |

---

# Important Exam Point

IP-based routing works using:

```text
CIDR collections
```

---

# Mental Model

```text
Client IP
   ↓
Route 53 checks matching CIDR
   ↓
Returns corresponding endpoint
```

---

# One-Line Revision

> IP-based routing directs users to endpoints based on matching client IP CIDR ranges.

- ![alt text](image-178.png)


# Route 53 Multi-Value Routing — Key Points

# What is Multi-Value Routing?

Returns:

```text
Multiple healthy endpoints
```

for same DNS query.

---

# Important Concept

Acts like:

```text
Client-side load balancing
```

Clients receive multiple IPs and choose one.

---

# NOT a Replacement for ELB

❌ No true load balancer behavior  
❌ No traffic proxying

Unlike ALB:

```text
Traffic does NOT pass through Route 53
```

---

# Main Advantage

Supports:

✅ health checks

Only healthy endpoints returned.

---

# Maximum Records Returned

```text
Up to 8 healthy records
```

per DNS response.

---

# Example

```text
multi.example.com
    ↓
Returns:
- 1.1.1.1
- 2.2.2.2
- 3.3.3.3
```

Client chooses endpoint.

---

# Important Difference from Simple Routing

| Feature | Simple Routing | Multi-Value |
|---|---|---|
| Multiple IPs | Yes | Yes |
| Health Checks | ❌ No | ✅ Yes |
| Healthy Filtering | ❌ No | ✅ Yes |

---

# Example Architecture

```text
Route 53
   ↓
Returns healthy IPs only
   ↓
Client selects one
```

---

# Hands-On Example

Created 3 records:

- US-East
- Singapore
- EU-Central

All with health checks.

---

# Initial Result

`dig` returned:

```text
3 healthy IPs
```

---

# Failure Simulation

Made EU health check unhealthy using:

```text
Invert Health Status
```

---

# Result

`dig` now returned:

```text
Only 2 healthy IPs
```

Unhealthy endpoint automatically removed.

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Multi-Value Routing | Multiple healthy endpoints |
| Max Records | 8 |
| Health Checks | Supported |
| Client Chooses Endpoint | Yes |
| ELB Replacement | No |

---

# Mental Model

```text
Route 53
   ↓
Filters unhealthy endpoints
   ↓
Returns healthy IPs only
```

---

# One-Line Revision

> Multi-value routing returns multiple healthy endpoints to clients and performs client-side load distribution using Route 53 health checks.

- ![alt text](image-179.png)
- ![alt text](image-180.png)


# Domain Registrar vs DNS Service — Key Points

# Important Distinction

| Service | Purpose |
|---|---|
| Domain Registrar | Buy/manage domain name |
| DNS Service | Manage DNS records |

---

# Domain Registrar

Used to:

```text
Purchase domain names
```

Example:

```text
example.com
```

Usually charged yearly.

---

# Common Registrars

- GoDaddy
- Google Domains
- Amazon Registrar

---

# DNS Service

Used to manage:

- A records
- CNAMEs
- NS records
- routing policies

Example DNS providers:

- Route 53
- Cloudflare
- GoDaddy DNS

---

# Important Concept

You can mix them.

Example:

```text
Buy domain from GoDaddy
Use Route 53 for DNS
```

Perfectly valid.

---

# How It Works

## Step 1

Buy domain from registrar.

Example:

```text
GoDaddy
```

---

# Step 2

Create:

```text
Public Hosted Zone
```

in Route 53.

---

# Step 3

Route 53 gives:

```text
NS (Name Server) records
```

Example:

```text
ns-123.awsdns.com
```

---

# Step 4

Update registrar settings to use:

```text
Route 53 name servers
```

---

# Result

```text
DNS queries now managed by Route 53
```

even though domain purchased elsewhere.

---

# Important Concept

Registrar answers:

```text
Who owns domain?
```

DNS service answers:

```text
What IP/service does domain point to?
```

---

# Example Flow

```text
Buy domain on GoDaddy
        ↓
Point NS records to Route 53
        ↓
Manage DNS entirely in AWS
```

---

# Important Exam Points

| Concept | Meaning |
|---|---|
| Registrar | Buys domain |
| DNS Service | Resolves domain |
| NS Records | Point to DNS provider |
| Hosted Zone | DNS records container |

---

# Mental Model

```text
Registrar = Owns address

DNS = Tells where address goes
```

---

# One-Line Revision

> A domain registrar manages domain ownership, while a DNS service like Route 53 manages how the domain resolves to resources.

- ![alt text](image-181.png)
- ![alt text](image-182.png)
- ![alt text](image-183.png)



# VPC Fundamentals

