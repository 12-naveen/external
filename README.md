https://chatgpt.com/c/699c0a1a-677c-8322-9223-67767598835b"

https://drive.google.com/drive/folders/1ftoye-rmQ-GpRvn7fyqU2RSsb36di9Tz

aws pass-HAPPYbirthday@123

https://drive.google.com/drive/folders/11jE6qe76oHyfS3RKTxGPr7BtxVnrHysk--tasks with photos

# CLOUD COMPUTING LAB RECORD

# WEEK 1 – EC2 INSTANCE CREATION AND SSH CONNECTION

## Aim

To create an EC2 instance and connect using SSH.

---

## Step 1: Login to AWS Console

1. Open browser
2. Login to AWS Management Console
3. Select region:

```
us-east-1
```

---

## Step 2: Open EC2 Dashboard

Search:

```
EC2
```

Click:

```
EC2 Dashboard
```

---

## Step 3: Launch EC2 Instance

Click:

```
Launch Instance
```

Fill:

Name:

```
Week1-EC2
```

AMI:

```
Ubuntu Server
```

Instance Type:

```
t2.micro
```

---

## Step 4: Create Key Pair

Click:

```
Create new key pair
```

Key Pair Name:

```
week1
```

Key Pair Type:

```
RSA
```

File Format:

```
.pem
```

Download key pair.

---

## Step 5: Configure Network Settings

Allow:

* SSH (22)

Source:

```
My IP
```

---

## Step 6: Launch Instance

Click:

```
Launch Instance
```

Output:
✔ EC2 instance created successfully.

---

## Step 7: Connect Using SSH

Open CMD.

Move to Downloads:

```bash
cd downloads
```

Run:

```bash
ssh -i "week1.pem" ubuntu@<Public-IP>
```

Type:

```
yes
```

Output:

```bash
ubuntu@ip-xxx:~$
```

---

## Final Output

✔ EC2 instance launched
✔ SSH connection successful

---

# WEEK 2 – EBS VOLUME MANAGEMENT

## Aim

To create, attach, partition, format, and mount an EBS volume.

---

## Step 1: Create EBS Volume

EC2 → Volumes → Create Volume

Size:

```
10 GB
```

Type:

```
gp3
```

Availability Zone:

```
us-east-1a
```

Click:

```
Create Volume
```

---

## Step 2: Attach Volume

Select volume → Actions → Attach Volume

Select instance.

Device Name:

```
/dev/sdf
```

Click:

```
Attach Volume
```

---

## Step 3: Connect to EC2

```bash
ssh -i "week2.pem" ec2-user@<Public-IP>
```

---

## Step 4: Check Attached Disk

```bash
lsblk
```

Output:

```bash
nvme1n1
```

---

## Step 5: Create Partition

```bash
sudo fdisk /dev/nvme1n1
```

Commands inside fdisk:

```
n
p
Enter
Enter
Enter
w
```

---

## Step 6: Refresh Partition Table

```bash
sudo partprobe
```

---

## Step 7: Format Partition

```bash
sudo mkfs.xfs /dev/nvme1n1p1
```

---

## Step 8: Create Mount Directory

```bash
sudo mkdir /mnt/naveen
```

---

## Step 9: Mount Volume

```bash
sudo mount /dev/nvme1n1p1 /mnt/naveen
```

---

## Step 10: Verify Mount

```bash
df -h
```

Output:
✔ Volume mounted successfully.

---

## Final Output

✔ EBS volume attached
✔ Partition created
✔ File system created
✔ Mounted successfully

---

# WEEK 3 – EFS (ELASTIC FILE SYSTEM)

## Aim

To create EFS and share storage between EC2 instances.

---

## Step 1: Create EFS

Search:

```
EFS
```

Click:

```
Create file system
```

Name:

```
Week3EFS
```

Click:

```
Create
```

---

## Step 2: Create EC2 Instances

Create:

* EFS1
* EFS2

Use:

```
Amazon Linux 2023
```

---

## Step 3: Connect to EFS1

```bash
ssh -i "key.pem" ec2-user@<Public-IP>
```

---

## Step 4: Install EFS Utilities

```bash
sudo yum install -y amazon-efs-utils
```

---

## Step 5: Create Mount Directory

```bash
sudo mkdir /efs
```

---

## Step 6: Mount EFS

```bash
sudo mount -t efs -o tls fs-xxxx:/ /efs
```

---

## Step 7: Repeat on EFS2

Mount same EFS on second instance.

---

## Step 8: Test Synchronization

On EFS1:

```bash
cd /efs
touch file1.txt
echo "Hello" > file1.txt
```

On EFS2:

```bash
cat /efs/file1.txt
```

Output:

```bash
Hello
```

---

## Final Output

✔ Shared file system working
✔ Files synchronized between EC2 instances

---

# WEEK 4 – AMAZON S3

## Aim

To create S3 bucket, enable public access, versioning, replication, and static website hosting.

---

# TASK 1 – Create Bucket and Public Access

## Step 1: Create Bucket

S3 → Create bucket

Name:

```
mybucket1-cclab
```

Disable:

```
Block all public access
```

Click:

```
Create bucket
```

---

## Step 2: Upload Object

Upload:

```
text.txt
```

---

## Step 3: Add Bucket Policy

Permissions → Bucket Policy

Paste:

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Principal":"*",
      "Action":"s3:GetObject",
      "Resource":"arn:aws:s3:::mybucket1-cclab/*"
    }
  ]
}
```

---

## Step 4: Test Public Access

Open:

```
Object URL
```

Output:
✔ File accessible publicly.

---

# TASK 2 – Enable Versioning

## Step 1

Bucket → Properties → Bucket Versioning → Enable

---

## Step 2

Upload same file again.

---

## Step 3

Enable:

```
Show versions
```

Output:
✔ Multiple versions visible.

---

# TASK 3 – Cross Region Replication

## Step 1

Create destination bucket in:

```
us-west-2
```

---

## Step 2

Enable versioning on both buckets.

---

## Step 3

Management → Replication Rules → Create Rule

---

## Step 4

Attempt IAM role creation.

Output:
❌ IAM permission denied due to lab restriction.

---

# TASK 4 – Static Website Hosting

## Step 1

Create bucket:

```
naveen-static-website
```

---

## Step 2

Enable:

```
Static Website Hosting
```

Index:

```
index.html
```

Error:

```
error.html
```

---

## Step 3

Upload:

* index.html
* error.html

---

## Step 4

Add bucket policy for public access.

---

## Step 5

Open:

```
Website endpoint URL
```

Output:
✔ Static website hosted successfully.

---

# WEEK 5 – VPC NETWORKING

## Aim

To create custom VPC with public and private subnets.

---

## Step 1: Create VPC

VPC → Create VPC

Name:

```
vpc1
```

CIDR:

```
10.0.0.0/16
```

---

## Step 2: Create Public Subnet

CIDR:

```
10.0.1.0/24
```

---

## Step 3: Create Private Subnet

CIDR:

```
10.0.2.0/24
```

---

## Step 4: Create Internet Gateway

Name:

```
Custom-IGW
```

Attach to VPC.

---

## Step 5: Create Public Route Table

Add route:

```
0.0.0.0/0 → IGW
```

Associate:

```
Public subnet
```

---

## Step 6: Create Private Route Table

Associate:

```
Private subnet
```

No IGW route.

---

## Step 7: Enable Auto Public IP

Subnet → Edit subnet settings

Enable:

```
Auto-assign public IPv4
```

---

## Step 8: Configure Security Group

Allow:

* SSH (22)
* HTTP (80)
* HTTPS (443)

---

## Step 9: Launch EC2 Instances

Public EC2:
✔ Public subnet

Private EC2:
✔ Private subnet

---

## Final Output

✔ Custom VPC created
✔ Public/private subnet configured
✔ Internet access verified
✔ Private subnet isolated

---

# WEEK 6 – BASTION SERVER & NAT GATEWAY

## Aim

To implement secure VPC using Bastion Server and NAT Gateway.

---

## Architecture

VPC:

```
10.0.0.0/16
```

Public Subnet:

```
10.0.1.0/24
```

Private Subnet:

```
10.0.2.0/24
```

---

## Step 1: Create VPC

Name:

```
Week6VPC
```

---

## Step 2: Create Subnets

Public:

```
PublicSubnet
```

Private:

```
PrivateSubnet
```

---

## Step 3: Create Internet Gateway

Name:

```
Week6IGW
```

Attach to VPC.

---

## Step 4: Create Public Route Table

Add:

```
0.0.0.0/0 → IGW
```

Associate:

```
PublicSubnet
```

---

## Step 5: Create Security Groups

### BastionSG

Allow:

```
SSH (22) → My IP
```

### DatabaseSG

Allow:

```
SSH (22) from 10.0.1.0/24
```

---

## Step 6: Launch Bastion Server

Public subnet
Public IP enabled.

---

## Step 7: Launch Database Server

Private subnet
Public IP disabled.

---

## Step 8: Copy DB Key

```bash
scp -i bastion.pem dbserver.pem ubuntu@<Bastion-IP>:/home/ubuntu/
```

---

## Step 9: Connect to Bastion

```bash
ssh -i bastion.pem ubuntu@<Bastion-IP>
```

---

## Step 10: Connect to Private DB

```bash
chmod 400 dbserver.pem
ssh -i dbserver.pem ubuntu@10.0.2.x
```

---

## Step 11: Verify No Internet

```bash
sudo apt update
```

Output:
❌ Fails

---

## Step 12: Create NAT Gateway

Name:

```
Week6NAT
```

Allocate Elastic IP.

---

## Step 13: Create Private Route Table

Add:

```
0.0.0.0/0 → NAT Gateway
```

Associate:

```
PrivateSubnet
```

---

## Step 14: Verify Internet

```bash
sudo apt update
```

Output:
✔ Works successfully

---

## Final Output

✔ Bastion access working
✔ Private subnet secured
✔ NAT internet access enabled


====================================================================================================================
=========================================================================================================

# WEEK 2 – EBS VOLUME MANAGEMENT

## Aim

To create, attach, partition, format, and mount an EBS volume in AWS EC2.

---

# THEORY

Amazon EBS (Elastic Block Store) provides persistent block-level storage volumes for EC2 instances. EBS volumes can be attached to EC2 instances and used like hard disks. They support partitioning, formatting, mounting, snapshots, and persistence.

---

# REQUIREMENTS

* AWS Account
* EC2 Instance running
* Key pair (.pem file)
* Internet connectivity

---

# PART A – CREATE EC2 INSTANCE

## Step 1: Login to AWS Console

1. Open browser
2. Login to AWS Management Console
3. Select region:

```
us-east-1
```

---

## Step 2: Open EC2 Dashboard

Search:

```
EC2
```

Click:

```
EC2 Dashboard
```

---

## Step 3: Launch EC2 Instance

Click:

```
Launch Instance
```

Fill:

Name:

```
Week2-Instance
```

AMI:

```
Amazon Linux 2023
```

Instance Type:

```
t2.micro
```

---

## Step 4: Create Key Pair

Click:

```
Create new key pair
```

Name:

```
week2
```

Key Pair Type:

```
RSA
```

File Format:

```
.pem
```

Download key pair.

---

## Step 5: Configure Security Group

Allow:

```
SSH (22)
```

Source:

```
My IP
```

---

## Step 6: Launch Instance

Click:

```
Launch Instance
```

Output:
✔ EC2 instance launched successfully.

---

# PART B – CREATE EBS VOLUME

## Step 7: Open Volumes Section

EC2 → Elastic Block Store → Volumes

Click:

```
Create Volume
```

---

## Step 8: Configure Volume

Size:

```
10 GB
```

Volume Type:

```
gp3
```

Availability Zone:

```
us-east-1a
```

Important:
The EBS volume and EC2 instance must be in the same Availability Zone.

Click:

```
Create Volume
```

Output:
✔ EBS volume created successfully.

---

# PART C – ATTACH EBS VOLUME

## Step 9: Attach Volume to EC2

1. Select created volume
2. Click:

```
Actions → Attach Volume
```

3. Select EC2 instance

Device Name:

```
/dev/sdf
```

Click:

```
Attach Volume
```

Output:
✔ Volume attached successfully.

---

# PART D – CONNECT TO EC2 INSTANCE

## Step 10: Open Command Prompt

Move to Downloads folder:

```bash
cd downloads
```

---

## Step 11: Connect Using SSH

```bash
ssh -i "week2.pem" ec2-user@<Public-IP>
```

Type:

```
yes
```

Output:

```bash
[ec2-user@ip-xxx ~]$
```

---

# PART E – VERIFY ATTACHED DISK

## Step 12: Check Storage Devices

```bash
lsblk
```

Expected Output:

```bash
nvme0n1
nvme1n1
```

Where:

* nvme0n1 → Root volume
* nvme1n1 → Newly attached EBS volume

---

# PART F – CREATE PARTITION

## Step 13: Open fdisk Utility

```bash
sudo fdisk /dev/nvme1n1
```

---

## Step 14: Create New Partition

Type commands one by one:

```bash
n
```

```bash
p
```

Press:

```bash
Enter
```

for:

* Partition number
* First sector
* Last sector

Finally type:

```bash
w
```

Output:
✔ Partition table written successfully.

---

# PART G – REFRESH PARTITION TABLE

## Step 15: Refresh Kernel Information

```bash
sudo partprobe
```

---

## Step 16: Verify Partition

```bash
lsblk
```

Expected Output:

```bash
nvme1n1p1
```

Output:
✔ Partition created successfully.

---

# PART H – FORMAT PARTITION

## Step 17: Create File System

```bash
sudo mkfs.xfs /dev/nvme1n1p1
```

Output:
✔ XFS file system created successfully.

---

# PART I – CREATE MOUNT DIRECTORY

## Step 18: Create Directory

```bash
sudo mkdir /mnt/naveen
```

Output:
✔ Mount directory created.

---

# PART J – MOUNT EBS VOLUME

## Step 19: Mount Volume

```bash
sudo mount /dev/nvme1n1p1 /mnt/naveen
```

Output:
✔ Volume mounted successfully.

---

# PART K – VERIFY MOUNTING

## Step 20: Check Mounted File Systems

```bash
df -h
```

Expected Output:

```bash
/dev/nvme1n1p1  10G  ...  /mnt/naveen
```

Output:
✔ EBS volume mounted successfully.

---

# PART L – TEST STORAGE

## Step 21: Create Test File

```bash
cd /mnt/naveen
sudo touch testfile.txt
ls
```

Expected Output:

```bash
testfile.txt
```

Output:
✔ File created successfully inside EBS volume.

---

# PART M – CREATE SNAPSHOT

## Step 22: Create Snapshot

1. Go to EC2 Console
2. Click:

```
Volumes
```

3. Select attached volume
4. Click:

```
Actions → Create Snapshot
```

Description:

```
Week2 Snapshot
```

Click:

```
Create Snapshot
```

Output:
✔ Snapshot created successfully.

---

# PART N – CROSS REGION SNAPSHOT COPY

## Step 23: Copy Snapshot

1. Open Snapshots
2. Select snapshot
3. Click:

```
Actions → Copy Snapshot
```

Destination Region:

```
us-east-1
```

Important:
In AWS Academy labs, IAM restrictions may block snapshot copy.

Possible Error:

```bash
Not authorized to perform ec2:CopySnapshot
```

Reason:
Lab IAM policy restriction.

---

# PART O – DETACH AND DELETE VOLUME

## Step 24: Unmount Volume

```bash
sudo umount /mnt/naveen
```

---

## Step 25: Detach Volume

1. EC2 → Volumes
2. Select volume
3. Click:

```
Actions → Detach Volume
```

Output:
✔ Volume detached successfully.

---

## Step 26: Delete Volume

Click:

```
Actions → Delete Volume
```

Output:
✔ Volume deleted successfully.

---

# VIVA QUESTIONS

## 1. What is Amazon EBS?

Amazon Elastic Block Store is a persistent block storage service used with EC2 instances.

---

## 2. What is the use of EBS?

It provides additional storage for EC2 instances.

---

## 3. What is the difference between EBS and Instance Store?

EBS is persistent storage while Instance Store is temporary.

---

## 4. What is IOPS?

IOPS stands for Input/Output Operations Per Second. It measures disk read/write performance.

---

## 5. What is gp3 volume?

A general-purpose SSD volume with configurable IOPS and throughput.

---

## 6. Why should EBS and EC2 be in same Availability Zone?

Because EBS volumes can only attach to EC2 instances within the same Availability Zone.

---

## 7. What is a Snapshot?

A backup copy of an EBS volume stored in Amazon S3.

---

# FINAL OUTPUT

✔ EC2 instance launched successfully
✔ EBS volume created successfully
✔ Volume attached to EC2
✔ Partition created using fdisk
✔ File system formatted using XFS
✔ Volume mounted successfully
✔ Snapshot created successfully
✔ Storage verified successfully
============================================================================================
=========================================================================================================
# WEEK 3 – AWS EFS (ELASTIC FILE SYSTEM)

## Aim

To create and configure Amazon Elastic File System (EFS) and mount it on multiple EC2 instances for shared storage access.

---

# THEORY

Amazon EFS (Elastic File System) is a scalable, elastic, fully managed network file system used with AWS Cloud services and on-premises resources. It allows multiple EC2 instances to access shared files simultaneously.

EFS uses the NFS (Network File System) protocol and automatically scales storage capacity.

---

# REQUIREMENTS

* AWS Account
* Two EC2 instances
* Security group allowing NFS (2049)
* Key pair (.pem file)
* Internet connectivity

---

# PART A – LOGIN TO AWS CONSOLE

## Step 1: Login

1. Open browser
2. Login to AWS Management Console
3. Select region:

```
us-east-1
```

---

# PART B – CREATE EFS FILE SYSTEM

## Step 2: Open EFS Dashboard

Search:

```
EFS
```

Click:

```
Elastic File System
```

---

## Step 3: Create File System

Click:

```
Create file system
```

Enter:

Name:

```
Week3EFS
```

VPC:
Select default VPC or custom VPC.

Click:

```
Create
```

Output:
✔ EFS file system created successfully.

---

# PART C – CREATE SECURITY GROUP

## Step 4: Configure Security Group

Go to:

```
EC2 → Security Groups
```

Create security group:

Name:

```
EFSSG
```

Add inbound rules:

### SSH Rule

Type:

```
SSH
```

Port:

```
22
```

Source:

```
My IP
```

---

### NFS Rule

Type:

```
NFS
```

Port:

```
2049
```

Source:

```
Anywhere
```

Click:

```
Create Security Group
```

Output:
✔ Security group created successfully.

---

# PART D – CREATE TWO EC2 INSTANCES

## Step 5: Launch First EC2 Instance

EC2 → Launch Instance

Name:

```
EFS1
```

AMI:

```
Amazon Linux 2023
```

Instance Type:

```
t2.micro
```

Security Group:

```
EFSSG
```

Key Pair:
Select existing key pair.

Click:

```
Launch Instance
```

---

## Step 6: Launch Second EC2 Instance

Repeat same steps.

Name:

```
EFS2
```

Security Group:

```
EFSSG
```

Click:

```
Launch Instance
```

Output:
✔ Two EC2 instances launched successfully.

---

# PART E – CONNECT TO FIRST EC2 INSTANCE

## Step 7: Open Command Prompt

Move to Downloads folder:

```bash
cd downloads
```

---

## Step 8: Connect Using SSH

```bash
ssh -i "keypair.pem" ec2-user@<Public-IP>
```

Type:

```
yes
```

Output:

```bash
[ec2-user@ip-xxx ~]$
```

---

# PART F – INSTALL EFS UTILITIES

## Step 9: Become Root User

```bash
sudo su
```

---

## Step 10: Install amazon-efs-utils

```bash
yum install -y amazon-efs-utils
```

Output:
✔ amazon-efs-utils installed successfully.

---

# PART G – CREATE MOUNT DIRECTORY

## Step 11: Create Directory

```bash
mkdir /efs
```

Output:
✔ Mount directory created.

---

# PART H – MOUNT EFS

## Step 12: Mount EFS File System

```bash
mount -t efs -o tls fs-xxxx:/ /efs
```

Replace:

```
fs-xxxx
```

with your EFS File System ID.

Output:
✔ EFS mounted successfully.

---

# PART I – REPEAT ON SECOND EC2 INSTANCE

## Step 13: Connect to Second Instance

```bash
ssh -i "keypair.pem" ec2-user@<Public-IP>
```

---

## Step 14: Install EFS Utilities

```bash
sudo su
yum install -y amazon-efs-utils
mkdir /efs
```

---

## Step 15: Mount Same EFS

```bash
mount -t efs -o tls fs-xxxx:/ /efs
```

Output:
✔ Same EFS mounted on second EC2 instance.

---

# PART J – VERIFY SHARED STORAGE

## Step 16: Create File on EFS1

On first instance:

```bash
cd /efs
touch file1.txt
echo "Hello from EFS1" > file1.txt
ls
```

Expected Output:

```bash
file1.txt
```

---

## Step 17: Verify File on EFS2

On second instance:

```bash
cd /efs
ls
```

Expected Output:

```bash
file1.txt
```

---

## Step 18: Verify File Content

```bash
cat file1.txt
```

Expected Output:

```bash
Hello from EFS1
```

Output:
✔ File synchronization successful.

---

# PART K – TEST REVERSE SYNCHRONIZATION

## Step 19: Create File on EFS2

```bash
touch file2.txt
echo "Hello from EFS2" > file2.txt
```

---

## Step 20: Verify on EFS1

```bash
ls
cat file2.txt
```

Expected Output:

```bash
Hello from EFS2
```

Output:
✔ Bidirectional synchronization successful.

---

# PART L – VERIFY MOUNTED FILE SYSTEM

## Step 21: Check Mounted File Systems

```bash
df -h
```

Expected Output:

```bash
fs-xxxx:/   mounted on /efs
```

Output:
✔ EFS mounted successfully.

---

# PART M – OPTIONAL PERSISTENT MOUNT

## Step 22: Edit fstab File

```bash
sudo nano /etc/fstab
```

Add:

```bash
fs-xxxx:/ /efs efs defaults,_netdev 0 0
```

Save file.

Purpose:
Automatically mounts EFS after reboot.

---

# VIVA QUESTIONS

## 1. What is Amazon EFS?

Amazon Elastic File System is a scalable network file system used for shared storage among multiple EC2 instances.

---

## 2. What protocol does EFS use?

EFS uses NFS (Network File System) protocol.

---

## 3. What is the advantage of EFS?

Multiple EC2 instances can access shared storage simultaneously.

---

## 4. What is the default NFS port number?

```bash
2049
```

---

## 5. Difference between EBS and EFS?

* EBS attaches to one EC2 instance.
* EFS can attach to multiple EC2 instances simultaneously.

---

## 6. Is EFS scalable?

Yes, EFS automatically scales storage capacity.

---

## 7. What is the use of amazon-efs-utils?

It helps mount and manage EFS file systems easily.

---

# FINAL OUTPUT

✔ EFS file system created successfully
✔ Two EC2 instances launched
✔ EFS mounted on both instances
✔ Shared storage verified successfully
✔ File synchronization working correctly
✔ Network file system implemented successfully

=====================================================================================
===================================================================================================
# WEEK 4 – AMAZON S3 STORAGE SERVICES

## Aim

To create and configure Amazon S3 buckets, enable public access, versioning, cross-region replication, and static website hosting.

---

# THEORY

Amazon S3 (Simple Storage Service) is an object storage service provided by AWS used for storing and retrieving files over the internet. S3 provides high scalability, durability, security, versioning, backup, replication, and static website hosting.

---

# REQUIREMENTS

* AWS Account
* Internet connectivity
* Browser access
* Sample files for upload

---

# TASK 1 – CREATE S3 BUCKET, UPLOAD OBJECT, AND ENABLE PUBLIC ACCESS

---

## Step 1: Login to AWS Console

1. Open browser
2. Login to AWS Management Console
3. Select region:

```
us-east-1
```

---

## Step 2: Open S3 Dashboard

Search:

```
S3
```

Click:

```
Amazon S3
```

---

## Step 3: Create S3 Bucket

Click:

```
Create bucket
```

Bucket Name:

```
mybucket1-cclab
```

Important:
Bucket name must be globally unique.

Region:

```
us-east-1
```

---

## Step 4: Disable Block Public Access

Uncheck:

```
Block all public access
```

Tick acknowledgment checkbox.

Click:

```
Create bucket
```

Output:
✔ Bucket created successfully.

---

## Step 5: Upload Object

1. Open created bucket
2. Click:

```
Upload
```

3. Click:

```
Add files
```

4. Select:

```
text.txt
```

5. Click:

```
Upload
```

Output:
✔ File uploaded successfully.

---

## Step 6: Add Bucket Policy for Public Access

Go to:

```
Permissions → Bucket Policy
```

Paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mybucket1-cclab/*"
    }
  ]
}
```

Click:

```
Save changes
```

Output:
✔ Public read access enabled.

---

## Step 7: Test Public Access

1. Open uploaded object
2. Copy:

```
Object URL
```

3. Open URL in browser.

Expected Output:
✔ File accessible publicly.

---

# TASK 2 – ENABLE VERSIONING AND OBSERVE OBJECT VERSIONS

---

## Step 8: Enable Bucket Versioning

Go to:

```
Bucket → Properties
```

Scroll to:

```
Bucket Versioning
```

Click:

```
Edit
```

Select:

```
Enable
```

Click:

```
Save changes
```

Output:
✔ Versioning enabled successfully.

---

## Step 9: Modify Existing File

Open local file:

```
text.txt
```

Add content:

```bash
Version 2 - Updated Content
```

Save file.

---

## Step 10: Upload Same File Again

1. Open bucket
2. Click:

```
Upload
```

3. Upload same file again.

Output:
✔ New object version created.

---

## Step 11: View Versions

Enable:

```
Show versions
```

Expected Output:

* Latest version
* Previous version
* Unique Version IDs

Output:
✔ Multiple versions visible successfully.

---

# TASK 3 – CONFIGURE CROSS-REGION REPLICATION (CRR)

---

## Step 12: Create Destination Bucket

Click:

```
Create bucket
```

Bucket Name:

```
mybucket1-cclab-replica
```

Region:

```
us-west-2
```

Click:

```
Create bucket
```

Output:
✔ Replica bucket created successfully.

---

## Step 13: Enable Versioning on Destination Bucket

Go to:

```
Properties → Bucket Versioning
```

Enable versioning.

Output:
✔ Versioning enabled on destination bucket.

---

## Step 14: Configure Replication Rule

Go to source bucket:

```
Management → Replication Rules
```

Click:

```
Create replication rule
```

Rule Name:

```
crr-rule
```

Apply to:

```
Entire bucket
```

Destination Bucket:

```
mybucket1-cclab-replica
```

---

## Step 15: IAM Role Configuration

Choose:

```
Create new role
```

Possible Error:

```bash
You don't have permission to create an IAM role
```

Reason:
AWS Academy lab restriction.

Output:
❌ Cross-region replication partially completed due to IAM restrictions.

---

# TASK 4 – CONFIGURE STATIC WEBSITE HOSTING

---

## Step 16: Create Website Bucket

Click:

```
Create bucket
```

Bucket Name:

```
naveen-static-website
```

Disable:

```
Block all public access
```

Click:

```
Create bucket
```

Output:
✔ Website bucket created successfully.

---

## Step 17: Enable Static Website Hosting

Go to:

```
Properties → Static website hosting
```

Click:

```
Edit
```

Enable:

```
Static website hosting
```

Index Document:

```
index.html
```

Error Document:

```
error.html
```

Click:

```
Save changes
```

Output:
✔ Static website hosting enabled.

---

## Step 18: Create HTML Files

Create:

### index.html

```html
<html>
<head>
<title>My Website</title>
</head>
<body>
<h1>Hello from S3 Static Website</h1>
</body>
</html>
```

---

### error.html

```html
<html>
<body>
<h1>Error Page</h1>
</body>
</html>
```

---

## Step 19: Upload HTML Files

Upload:

* index.html
* error.html

Output:
✔ HTML files uploaded successfully.

---

## Step 20: Add Bucket Policy for Website Access

Go to:

```
Permissions → Bucket Policy
```

Paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadWebsite",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::naveen-static-website/*"
    }
  ]
}
```

Click:

```
Save changes
```

Output:
✔ Website public access enabled.

---

## Step 21: Test Static Website

Go to:

```
Properties → Static Website Hosting
```

Copy:

```
Website endpoint URL
```

Open URL in browser.

Expected Output:

```html
Hello from S3 Static Website
```

Output:
✔ Static website hosted successfully.

---

# VIVA QUESTIONS

## 1. What is Amazon S3?

Amazon S3 is an object storage service used for storing and retrieving files over the internet.

---

## 2. What is Bucket in S3?

A bucket is a container used to store objects/files in S3.

---

## 3. What is Versioning?

Versioning stores multiple versions of the same object.

---

## 4. What is Cross-Region Replication?

CRR automatically copies objects from one bucket to another bucket in a different AWS region.

---

## 5. What is Static Website Hosting?

S3 can host static websites using HTML, CSS, and JavaScript files.

---

## 6. Why was CRR not fully configured?

Due to IAM role creation restrictions in AWS Academy lab environment.

---

## 7. What is the use of Bucket Policy?

Bucket policy controls access permissions to S3 resources.

---

# FINAL OUTPUT

✔ S3 bucket created successfully
✔ Public object access enabled
✔ Bucket versioning enabled
✔ Multiple object versions verified
✔ Cross-region replication attempted
✔ Static website hosting configured successfully
✔ Website accessible through S3 endpoint
=======================================================================================
===========================================================================================
# WEEK 5 – AWS VPC NETWORKING

## Aim

To create and configure a custom AWS Virtual Private Cloud (VPC) with public and private subnets, internet gateway, route tables, security groups, and EC2 instances.

---

# THEORY

Amazon VPC (Virtual Private Cloud) is a logically isolated virtual network in AWS where users can launch AWS resources securely. VPC allows complete control over networking components such as subnets, route tables, internet gateways, security groups, and network ACLs.

A public subnet provides internet access using an Internet Gateway, while a private subnet restricts direct internet connectivity.

---

# REQUIREMENTS

* AWS Account
* Internet connectivity
* EC2 access
* Key pair (.pem file)

---

# PART A – LOGIN TO AWS CONSOLE

## Step 1: Login

1. Open browser
2. Login to AWS Management Console
3. Select region:

```
us-east-1
```

---

# PART B – CREATE CUSTOM VPC

## Step 2: Open VPC Dashboard

Search:

```
VPC
```

Click:

```
VPC Dashboard
```

---

## Step 3: Create VPC

Click:

```
Create VPC
```

Choose:

```
VPC only
```

Enter:

Name:

```
vpc1
```

IPv4 CIDR:

```
10.0.0.0/16
```

Tenancy:

```
Default
```

Click:

```
Create VPC
```

Output:
✔ Custom VPC created successfully.

---

# PART C – CREATE PUBLIC AND PRIVATE SUBNETS

## Step 4: Create Public Subnet

Go to:

```
VPC → Subnets
```

Click:

```
Create subnet
```

Select VPC:

```
vpc1
```

Enter:

Subnet Name:

```
subnet1
```

Availability Zone:

```
us-east-1a
```

CIDR Block:

```
10.0.1.0/24
```

Click:

```
Create subnet
```

---

## Step 5: Create Private Subnet

Click:

```
Create subnet
```

Select VPC:

```
vpc1
```

Enter:

Subnet Name:

```
subnet2
```

Availability Zone:

```
us-east-1a
```

CIDR Block:

```
10.0.2.0/24
```

Click:

```
Create subnet
```

Output:
✔ Public and private subnets created successfully.

---

# PART D – CREATE INTERNET GATEWAY

## Step 6: Create Internet Gateway

Go to:

```
VPC → Internet Gateways
```

Click:

```
Create internet gateway
```

Enter:

Name:

```
Custom-IGW
```

Click:

```
Create internet gateway
```

---

## Step 7: Attach Internet Gateway

Select gateway.

Click:

```
Actions → Attach to VPC
```

Select:

```
vpc1
```

Click:

```
Attach internet gateway
```

Output:
✔ Internet Gateway attached successfully.

---

# PART E – CREATE PUBLIC ROUTE TABLE

## Step 8: Create Public Route Table

Go to:

```
VPC → Route Tables
```

Click:

```
Create route table
```

Enter:

Name:

```
Public-RT
```

VPC:

```
vpc1
```

Click:

```
Create route table
```

---

## Step 9: Add Internet Route

Select:

```
Public-RT
```

Go to:

```
Routes → Edit routes
```

Add:

Destination:

```
0.0.0.0/0
```

Target:

```
Custom-IGW
```

Click:

```
Save changes
```

Output:
✔ Internet route added successfully.

---

## Step 10: Associate Public Subnet

Go to:

```
Subnet Associations → Edit subnet associations
```

Select:

```
subnet1
```

Click:

```
Save associations
```

Output:
✔ Public subnet associated successfully.

---

# PART F – CREATE PRIVATE ROUTE TABLE

## Step 11: Create Private Route Table

Click:

```
Create route table
```

Enter:

Name:

```
Private-RT
```

VPC:

```
vpc1
```

Click:

```
Create route table
```

---

## Step 12: Associate Private Subnet

Select:

```
Private-RT
```

Go to:

```
Subnet Associations → Edit subnet associations
```

Select:

```
subnet2
```

Click:

```
Save associations
```

Important:
Do not add Internet Gateway route.

Output:
✔ Private subnet configured successfully.

---

# PART G – ENABLE AUTO PUBLIC IP

## Step 13: Configure Public Subnet

Go to:

```
VPC → Subnets
```

Select:

```
subnet1
```

Click:

```
Actions → Edit subnet settings
```

Enable:

```
Auto-assign public IPv4 address
```

Click:

```
Save
```

Output:
✔ Auto public IP enabled successfully.

---

# PART H – CONFIGURE SECURITY GROUPS

## Step 14: Open Security Groups

Go to:

```
VPC → Security Groups
```

Important:
AWS Academy labs may restrict creation of new security groups.

If restricted:
Use existing default security group.

---

## Step 15: Configure Inbound Rules

Add rules:

### SSH

Type:

```
SSH
```

Port:

```
22
```

Source:

```
My IP
```

---

### HTTP

Type:

```
HTTP
```

Port:

```
80
```

Source:

```
Anywhere IPv4
```

---

### HTTPS

Type:

```
HTTPS
```

Port:

```
443
```

Source:

```
Anywhere IPv4
```

Click:

```
Save rules
```

Output:
✔ Security group configured successfully.

---

# PART I – LAUNCH PUBLIC EC2 INSTANCE

## Step 16: Launch Public EC2

Go to:

```
EC2 → Launch Instance
```

Enter:

Name:

```
Public-EC2
```

AMI:

```
Amazon Linux 2023
```

Instance Type:

```
t2.micro
```

Key Pair:
Select existing key pair.

---

## Step 17: Configure Network Settings

VPC:

```
vpc1
```

Subnet:

```
subnet1
```

Auto Assign Public IP:

```
Enable
```

Security Group:

```
Default SG
```

Click:

```
Launch Instance
```

Output:
✔ Public EC2 launched successfully.

---

# PART J – LAUNCH PRIVATE EC2 INSTANCE

## Step 18: Launch Private EC2

Click:

```
Launch Instance
```

Name:

```
Private-EC2
```

AMI:

```
Amazon Linux 2023
```

Instance Type:

```
t2.micro
```

Key Pair:
Select existing key pair.

---

## Step 19: Configure Private Network

VPC:

```
vpc1
```

Subnet:

```
subnet2
```

Auto Assign Public IP:

```
Disable
```

Security Group:

```
Default SG
```

Click:

```
Launch Instance
```

Output:
✔ Private EC2 launched successfully.

---

# PART K – TEST NETWORK CONNECTIVITY

## Step 20: Verify Public EC2

Connect using SSH:

```bash
ssh -i "keypair.pem" ec2-user@<Public-IP>
```

Test internet:

```bash
ping google.com
```

Expected Output:
✔ Internet connectivity successful.

---

## Step 21: Verify Private EC2

Check:

```
No public IP assigned
```

Expected Output:
✔ Direct public SSH access unavailable.

---

## Step 22: Verify Internal VPC Communication

From Public EC2:

```bash
ssh ec2-user@<Private-IP>
```

Expected Output:
✔ Internal communication successful.

---

# VIVA QUESTIONS

## 1. What is Amazon VPC?

Amazon VPC is a logically isolated virtual network in AWS.

---

## 2. What is a subnet?

A subnet is a smaller network inside a VPC.

---

## 3. Difference between Public and Private subnet?

* Public subnet has internet access through Internet Gateway.
* Private subnet does not have direct internet access.

---

## 4. What is an Internet Gateway?

An Internet Gateway allows communication between VPC and internet.

---

## 5. What is a Route Table?

A route table controls network traffic routing inside a VPC.

---

## 6. Why is Public EC2 accessible from internet?

Because it has public IP and route to Internet Gateway.

---

## 7. Why is Private EC2 secure?

Because it has no public IP and no direct internet route.

---

# FINAL OUTPUT

✔ Custom VPC created successfully
✔ Public and private subnets configured
✔ Internet Gateway attached successfully
✔ Route tables configured successfully
✔ Security groups configured
✔ Public EC2 launched with internet access
✔ Private EC2 isolated from public internet
✔ Internal VPC communication verified successfully
