# Solving Real-World Business Challenges with AWS

Reflecting on my AWS journey, I revisited a critical business challenge my previous employer faced: real-time data synchronization from multiple branch offices to the Head Office (HO).

## **Overview**
This guide walks through setting up **AWS DataSync** to synchronize files from a **local shared folder (branch office storage)** to **Amazon S3 (Head Office central storage).**

## **Problem Statement**
The organization required a centralized and real-time data synchronization solution for 9 branch offices and remote field users, ensuring:

  -  Offline caching with auto-sync when reconnected
  -  Data consistency, replication & fast reporting
  -  Secure & scalable data transfer

## **Key Challenges**

  - Real-time sync – Instantly update HO storage from all locations
  - Offline resilience – Ensure local caching & automatic re-sync
  - Bandwidth optimization – Efficiently handle large files
  - Scalability & availability – Future-proof for growing data
  - Security – Ensure secure access & controlled sync


## **AWS-Powered Resolution**

To solve this, I leveraged AWS DataSync to seamlessly sync on-premise file shares to AWS S3. This automated, scalable, and secure approach enabled:

  - Real-time file replication with minimal latency
  - Automatic sync upon reconnection from offline mode
  - Centralized storage in S3 for seamless reporting & analytics

## **Architecture Diagram**


![Data Synchronization System Architecture](Architecture%20Diagran/Data-Synchronization-System_Architecture.jpg)



## **AWS Services Used**

  - AWS DataSync : Automated and secure data transfer
  - Amazon S3 : Centralized storage for HO reporting and analytics
  - AWS IAM : Roles & permissions for secure access
  - Amazon CloudWatch : Monitoring and logging
---

## **Step 1: Create an S3 Bucket for Head Office**
1. Go to the **AWS S3 Console**: [S3 Console](https://s3.console.aws.amazon.com/s3/home)
2. Click **Create Bucket**.
3. Enter a **unique bucket name**:  
   ```
   company-ho-data-bucket
   ```
4. Choose a **region** (e.g., `us-east-1`).
5. **Enable Versioning** (optional but recommended).
6. **Set Permissions:**  
   - Block **public access**.
7. Click **Create Bucket**.

✅ **Your S3 bucket is ready!**



## **Step 3: Deploy a DataSync Agent (For Branch Office)**
Since your branch office data is stored locally, you need an **AWS DataSync Agent** to read & transfer files.


### **Option 2: Install DataSync Agent as a Virtual Machine**
(If branch office has an on-premise server)
1. Download the **DataSync Agent OVA** from the AWS Console.
2. Deploy it as a **VM** on **VMware**.
3. Power on the VM and note the **IP Address** displayed.

✅ **Your DataSync Agent is now running.**

---

## **Step 4: Register the DataSync Agent in AWS**
1. Go back to the **AWS DataSync Console**.
2. Click **Create Agent** → Choose **On-Premises** **.
3. Enter the **IP address** of your DataSync Agent.
4. Click **Create Agent**.

✅ **Your DataSync Agent is now registered!**

---

## **Step 5: Create the Source Location (Branch Office Shared Drive)**
Define **where DataSync will pull files from**.

1. In **AWS DataSync Console**, go to **Tasks** → Click **Create Task**.
2. In **Source Location**, click **Create Location**.
3. Choose your branch’s storage type:
   - branch uses **Windows shared drive (SMB)** → Choose **SMB**.
     

### **SMB (Windows Shared Folder)**
- Enter the **Branch Office File Server IP**.
- Enter the shared folder path:  
  ```
  \\192.168.1.10\branch-shared-folder
  ```
- Enter **Windows username & password**.



✅ **Your Source Location is now set up!**

---

## **Step 6: Create the Destination Location (Amazon S3)**
Define **where DataSync will send the files (Head Office S3 bucket).**

1. Under **Destination Location**, click **Create Location**.
2. Choose **Amazon S3**.
3. Select your bucket:  
   ```
   company-ho-data-bucket
   ```
4. Set the **S3 folder prefix** (e.g., `branch-1/`).
5. Click **Create Location**.

✅ **Your Destination Location is set!**

---

## **Step 7: Configure the DataSync Task**
1. Go back to **Create Task**.
2. Select the **Source Location** (branch shared drive).
3. Select the **Destination Location** (S3 bucket).
4. **Task Settings:**
   - Enable **"Transfer only changed files"** ✅
   - Enable **"Verify file integrity"** ✅
   - Set **Bandwidth Limits** (optional).
5. Click **Next**.

✅ **Your DataSync Task is ready!**

---

## **Step 8: Schedule Automatic Syncing**
1. Under **Schedule**, select:
   - **Every 15 minutes** (for near real-time sync).
   - **Hourly / Daily** (for lower bandwidth usage).
2. Click **Create Task**.

✅ **Your branch-to-HO sync is scheduled!**

---

## **Step 9: Start Sync & Monitor Logs**
1. Select your **DataSync Task**.
2. Click **Start Task Manually** (for testing).
3. Check **Task Logs** in **AWS CloudWatch** to monitor progress.

✅ **Your setup is complete!** 🎉

