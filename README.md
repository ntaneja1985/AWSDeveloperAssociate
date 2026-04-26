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
