# ☁ AWS & 🐧 Linux Introduction — Refined Notes

---

## 🌍 AWS Fundamentals

### 🗺 AWS Regions – The Global Backbone

*Definition:*
An *AWS Region* is a *geographically distinct area* that hosts multiple *Availability Zones (AZs)*.
Regions are independent, ensuring *fault isolation, data sovereignty, and minimal latency*.

| **Region Name**       | **Code**  | **Location**  |
| --------------------- | --------- | ------------- |
| US East (N. Virginia) | us-east-1 | North America |

> Each region operates its own infrastructure — EC2, RDS, S3, Lambda, and more.

---

### 🏢 Availability Zones (AZs)

* Each region contains *2–6 AZs* for redundancy.
* An *AZ* = one or more *physically separate datacenters* linked by *high-speed, low-latency connections*.

*Example:*

```
Region: ap-south-1
├── ap-south-1a
├── ap-south-1b
└── ap-south-1c
```

> If one AZ fails, others continue to serve — ensuring high availability.

---

### 🌐 VPC (Virtual Private Cloud)

A *VPC* is an *isolated virtual network* where AWS resources run securely.
You can define:

* IP address ranges (CIDR blocks)
* Subnets
* Route tables
* Internet Gateways
* Security groups & NACLs

*Multiple VPCs can exist per region for separation of environments.*

---

### 🧱 Subnets in AWS

*Definition:*
A *Subnet* is a logical segment of a VPC. Each subnet exists in a *single AZ* and is categorized as *public* or *private*.

#### ✅ Best Practices

1. Use multiple AZs for high availability
2. Separate workloads into public/private tiers
3. Provide NAT Gateway access for private subnets
4. Reserve CIDR ranges for future expansion
5. Use consistent tagging (e.g., Env=Prod, Tier=App)
6. Manage access with Route Tables and Security Groups

---

## ⚙ AWS EC2 — Key Configuration Fields

### 1. 🏷 Name & Tags

* Tags help identify and manage instances.
* Example: `Name=WebServer`, `Environment=Prod`.

---

### 2. 💽 Application & OS Images (AMI)

* *AMI* = Pre-configured OS image with optional software.
* Choose from Amazon Linux, Ubuntu, or Windows.
* *Free Tier eligible* images help reduce cost.
* Sources: Quick Start, Marketplace, or Community AMIs.

---

### 3. ⚡ Instance Type

Defines *hardware specs* — CPU, RAM, and network performance.
Examples: `t3.micro`, `m5.large`.
*Free Tier* supports `t2.micro` or `t3.micro`.

> Match instance types to workloads: compute, memory, or general purpose.

---

### 4. 🔑 Key Pair (Login)

* Enables secure SSH (Linux) or RDP (Windows) access.
* Without a key pair, remote login may not be possible.

---

### 5. 🌐 Network Settings

Control how your instance connects within AWS.

Includes:

* **VPC/Subnet:** Network placement
* **Public IP:** Internet access
* **Security Groups:** Inbound/outbound firewall rules
* **Network Interfaces:** Multi-homing support

> Public subnets enable external access, private ones isolate workloads.

---

### 6. 💾 Storage Configuration

Each EC2 has a *root volume* (OS) and optional *EBS volumes* for extra storage.

* **Types:** gp3, gp2, io1, io2, st1, sc1
* **Size (GiB):** Disk capacity
* **IOPS/Throughput:** Performance parameters
* **Delete on Termination:** Auto-cleanup
* **Encryption:** Use KMS for security
* **Device Mapping:** `/dev/xvda`, `/dev/sdb`, etc.

---

### 7. ⚙ Advanced Settings

For permissions, startup automation, and control policies.

| **Field**              | **Purpose**                                     |
| ---------------------- | ----------------------------------------------- |
| IAM Role               | Grant AWS service access (e.g., S3, CloudWatch) |
| Shutdown Behavior      | Choose Stop or Terminate                        |
| Termination Protection | Prevent accidental deletion                     |
| Monitoring             | Enable detailed CloudWatch metrics              |
| Placement Group        | Control physical instance placement             |
| Tenancy                | Shared vs Dedicated hardware                    |
| User Data              | Run startup scripts (e.g., install packages)    |
| Boot Mode              | UEFI or Legacy BIOS                             |
| Metadata Options       | Configure IMDSv2                                |
| Licensing              | Apply software licenses                         |
| Tag Specs              | Inherit tags to EBS/Network interfaces          |

---

## 🐧 Linux Fundamentals

### 🧩 What is Linux?

An *open-source operating system* powering most servers, cloud workloads, and DevOps environments.
Known for stability, security, and performance.

---

### 🏗 Linux Architecture Overview

**Kernel:**
Handles CPU, memory, device I/O, and process scheduling.
Acts as the interface between hardware and user processes.

**Shell:**
Command-line interpreter connecting users to the kernel.
Common shells: `bash`, `zsh`, `sh`, `fish`.

**Why the Shell Matters:**

* Enables automation and scripting
* Powers configuration and deployment tasks
* Essential for DevOps workflows

---

## 🔤 1. Basic Linux Commands

| **Command** | **Purpose**            | **Example**                |
| ----------- | ---------------------- | -------------------------- |
| pwd         | Show current directory | `pwd`                      |
| cd          | Change directory       | `cd /home/user/Documents`  |
| mkdir       | Create directory       | `mkdir projects`           |
| rmdir       | Delete empty directory | `rmdir temp`               |
| touch       | Create file            | `touch index.html`         |
| cat         | View file contents     | `cat notes.txt`            |
| echo        | Output text            | `echo "Hello" > greet.txt` |
| history     | List recent commands   | `history`                  |

*🧪 Lab 1 – Basic Navigation*
Create a directory, add a text file, and print the path and history.

---

## 📁 2. File Management Commands

| **Command** | **Purpose**             | **Example**               |
| ----------- | ----------------------- | ------------------------- |
| ls          | List files/directories  | `ls -l`                   |
| cp          | Copy files              | `cp config.yaml /backup/` |
| mv          | Move or rename          | `mv old.txt new.txt`      |
| rm          | Delete files            | `rm -r temp_dir`          |
| find        | Search files            | `find . -name "*.log"`    |
| du          | Show disk usage         | `du -sh /var/log/`        |
| df          | Display free disk space | `df -h`                   |

*🧪 Lab 2 – File Operations*
Copy, move, delete, and check disk usage.

---

## ⚙ 3. Process Management Commands

| **Command** | **Purpose**              | **Example**      |
| ----------- | ------------------------ | ---------------- |
| ps          | Show running processes   | `ps -ef`         |
| top         | Monitor system activity  | `top`            |
| htop        | Interactive process view | `htop`           |
| kill        | Terminate process by PID | `kill -9 1234`   |
| jobs/bg/fg  | Manage jobs              | `bg %1`, `fg %1` |

*🧪 Lab 3 – Process Control*
Run, monitor, and kill background processes.

---

## 🔐 Permissions & Ownership

| **Command** | **Purpose**             | **Example**                 |
| ----------- | ----------------------- | --------------------------- |
| chmod       | Change file permissions | `chmod 755 script.sh`       |
| chown       | Change file owner       | `chown user:group file.txt` |
| chgrp       | Change group ownership  | `chgrp dev file.txt`        |

*🧪 Lab 4 – Ownership Management*
Identify file permissions, modify access, and log results in `/home/user/answers.txt`.

---

# ✅ Summary

### **AWS**

* *Region:* Global zone cluster
* *AZ:* Fault-tolerant data center group
* *VPC:* Secure virtual network
* *Subnet:* Logical segmentation within a VPC
* *EC2 Setup:* Define Name, AMI, Type, Key Pair, Network, Storage, Advanced options

### **Linux**

* Open-source OS core to DevOps
* Master file, process, and permission management
* Foundation for *cloud administration, automation,* and *infrastructure scripting*

> 🚀 **AWS + Linux = Cloud Powerhouse** — mastering both unlocks full control of scalable, automated environments.

---