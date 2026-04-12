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
