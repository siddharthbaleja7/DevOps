# AWS Cloud Infrastructure & Linux Systems Engineering

This documentation covers the core building blocks of cloud computing and the operating system that powers it. Understanding the marriage between AWS infrastructure and Linux internals is the "Day 1" requirement for any DevOps professional.

---

## ☁️ Section 1: AWS Global Infrastructure & Networking

AWS doesn't just run on "one big computer." It is a globally distributed network designed for 99.99% availability.

### 1.1 The Hierarchy of Availability
* **AWS Regions:** Geographic locations (like `ap-south-1` for Mumbai). Regions are isolated to ensure that a natural disaster in one part of the world doesn't affect another.
* **Availability Zones (AZs):** Each Region has multiple AZs. These are physically separate data centers connected by ultra-fast fiber. 
    * **Rule of Thumb:** Always deploy your app in at least **two AZs**. If AZ-A goes underwater, AZ-B keeps your business running.



### 1.2 Virtual Private Cloud (VPC) & Subnetting
A VPC is my private slice of the AWS cloud. Inside it, I use **Subnets** to organize my resources:
* **Public Subnets:** Connected to an **Internet Gateway (IGW)**. This is where I put Web Servers that the world needs to see.
* **Private Subnets:** No direct path from the internet. This is for Databases and Backend logic.
* **The Secret to Privacy:** We use **NAT Gateways** so that private instances can download updates from the web without letting the web see them.

---

## 🖥️ Section 2: Mastering EC2 (Elastic Compute Cloud)

Launching an EC2 instance isn't just "clicking a button." It requires a technical blueprint:

1.  **AMI (The Software Blueprint):** Selecting the OS. I prefer **Amazon Linux 2023** for AWS-native features or **Ubuntu** for broad community support.
2.  **Instance Families:** * **t-series (t2/t3.micro):** "Burstable" performance, great for learning.
    * **m-series:** General purpose.
    * **c-series:** Compute-optimized for heavy processing.
3.  **Storage (EBS - Elastic Block Store):** * **gp3:** The latest general-purpose SSD. It's cheaper and faster than gp2.
    * **Snapshotting:** I can take a "point-in-time" backup of my drive before making risky changes.
4.  **Security Groups (The Firewall):** This is a **Stateful** firewall. I only open the ports I need (Port 22 for SSH, 80 for HTTP, 443 for HTTPS).
5.  **User Data (Bootstrap Scripts):** I use this to automate my setup. 
    * *Example:* I can write a script to `yum install httpd` automatically at launch so the web server is ready before I even log in.



---

## 🐧 Section 3: Linux Systems & Shell Essentials

Linux is the "Heart" of DevOps. If you can't navigate the terminal, you can't automate the cloud.

### 3.1 The Architecture
* **The Kernel:** The "Brain" that talks to the hardware (CPU/RAM).
* **The Shell (Bash):** My translator. I give it commands; it tells the Kernel what to do.

### 3.2 The "DevOps Choice" Command Reference
I’ve categorized these by how I actually use them in a real environment:

#### **File System Navigation & Manipulation**
* `pwd` & `ls -lah`: Essential for knowing where I am and seeing "hidden" config files (like `.bashrc`).
* `mkdir -p /path/to/dir`: I use the `-p` flag to create an entire directory tree in one go.
* `touch` & `cat`: Creating and viewing files quickly.
* `tail -f /var/log/messages`: **Pro-Tip:** I use this to watch system logs in real-time to debug crashes.

#### **File Management & Operations**
* `cp -rv`: Copying with "Verbose" mode so I can see exactly what is moving.
* `find / -name "*.conf"`: Finding that one configuration file hidden in the system.
* `du -sh *`: Checking which folder is eating up all my disk space.

#### **Process & Performance Management**
In DevOps, we often have to find and kill "Zombie" processes:
* `top` / `htop`: The "Task Manager" for Linux.
* `ps aux | grep java`: Checking if my Jenkins or Java app is actually running.
* `kill -9 <PID>`: The "Nuclear" option to stop a process that refuses to close.
* `&` and `bg / fg`: Running long scripts in the background so I can keep using my terminal.

### 3.3 Permissions & Ownership
One of the biggest hurdles in Linux is `Permission Denied`.
* **`chmod 400 my-key.pem`**: I learned that AWS will reject my SSH key if the permissions are too "open."
* **`chown user:group`**: Essential for making sure the Web Server (Nginx/Apache) has the right to read my website files.

---
