# Task 1: Explore Your Pre-Provisioned Resource Group

**Difficulty:** Beginner  
**Estimated Time:** 15–20 Minutes  
**Type:** Read Only (No changes will be made)

---

##  Objective

In this task, you will log in to the Azure Portal for the first time and explore the Resource Group that has already been set up for you. You will learn what a Resource Group is, what it contains, and how tags are used in Azure.

---

##  Pre-Requisites

Before starting this task, make sure you have the following ready from your **Lab Details Page**:

| Item | Where to Find It |
|---|---|
| Azure Portal Login URL | Lab Details Page |
| Username | Lab Details Page |
| Password | Lab Details Page |
| Your Resource Group Name | Lab Details Page (e.g., `rg-participant-john`) |

---

##  Step-by-Step Instructions

---

### Step 1: Open the Azure Portal

1. Open any web browser on your computer (Chrome, Edge, or Firefox recommended)
2. In the address bar at the top, type the following URL and press **Enter**:

```
https://portal.azure.com
```

3. The Microsoft Azure login page will appear

---

<img src="">
---

### Step 2: Enter Your Username

1. On the login screen, you will see a box that says **"Email, phone, or Skype"**
2. Type your **Username** exactly as it appears on your Lab Environment Page
3. Click the **Next** button

---

### Step 3: Enter Your Password

1. A new screen will appear asking for your **Password**
2. Type your **Password** exactly as shown on your Lab Environments Page
3. Click the **Sign in** button

---

<img src="">

---

### Step 4: Handle the "Stay Signed In?" Prompt

1. After signing in, a pop-up may appear asking **"Stay signed in?"**
2. Click **No** (recommended for lab environments)
3. You will now be taken to the **Azure Portal Home Page**

---

### Step 5: Recognize the Azure Portal Home Page

1. You should now see the **Azure Home Page**
2. You will see icons/tiles for services like Virtual Machines, Storage Accounts, Resource Groups, etc.
3. At the top, you will see a **Search Bar** — this is the main way to navigate Azure
4. On the left side, there is a **sidebar menu** with quick links

---

<img src="">

---

### Step 6: Navigate to Resource Groups

1. Click on the **Search Bar** at the very top of the page (it says *"Search resources, services, and docs"*)
2. Type: `Resource Groups`
3. In the dropdown results that appear, click on **Resource Groups** (it will have a blue folder icon)

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the search bar with "Resource Groups" typed and the dropdown result highlighted]**

---

### Step 7: Find Your Assigned Resource Group

1. The **Resource Groups** page will open showing a list of resource groups
2. Look through the list and find your resource group name from your Lab Details Page
   - Example: `rg-participant-john`
3. Click on your **Resource Group name** to open it

>  **Note:** You may see only one resource group in the list — that is yours. Do not click on any other resource group if multiple are visible.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Resource Groups list with the participant's RG highlighted and arrow pointing to it]**

---

### Step 8: Explore the Overview Tab

1. After clicking on your Resource Group, you will land on the **Overview** tab automatically
2. Look at the information displayed on this page and note down the following:

| Field | What to Look For | Your Value |
|---|---|---|
| **Subscription** | Name of the Azure subscription | |
| **Region** | Location where the RG is hosted (e.g., East US) | |
| **Resources count** | Number of resources inside the RG | |

3. In the **center of the page**, you will see a list of **Resources** — these are everything inside your RG
4. You should see at least one resource — the **Linux Virtual Machine** that was pre-deployed for you

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Resource Group Overview tab with Subscription, Region, and Resources list labeled with arrows]**

---

### Step 9: Understand What You See in the Resources List

1. In the resources list, you will see columns: **Name**, **Type**, **Location**
2. Find the row where **Type** says `Virtual machine` — this is your Linux VM
3. You may also see related resources like:
   - **Network Interface** — the virtual network card for the VM
   - **Virtual Network** — the network the VM is connected to
   - **Public IP Address** — the IP used to reach the VM from internet
   - **OS Disk** — the storage disk attached to the VM
   - **Network Security Group** — the firewall rules for the VM

>  **What this means:** Even though you deployed "just a VM", Azure automatically created several supporting resources. All of them live together inside your Resource Group.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Resources list with each resource type labeled — VM, NIC, VNet, Public IP, Disk, NSG]**

---

### Step 10: Check the Tags on Your Resource Group

1. On the left-hand menu inside your Resource Group, scroll down and look for **Tags**
2. Click on **Tags**
3. You will see a list of key-value pairs that have been assigned to your RG, for example:

| Tag Name | Tag Value |
|---|---|
| Environment | Training |
| Owner | Your Name |
| CostCenter | Lab |

4. Read each tag and understand what it represents

> 💡 **What this means:** Tags are labels attached to Azure resources. In real companies, tags are used to track which department a resource belongs to, who owns it, and what environment it is in (Dev, Test, Prod). They are also used for **cost reporting** — filtering bills by department or project.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Tags section showing the key-value pairs for Environment, Owner, and CostCenter]**

---

### Step 11: Go Back to Overview

1. Click on **Overview** in the left menu to go back to the main Resource Group page
2. At the top of the page, click on your **Subscription name** (it appears as a clickable link)
3. This will open the Subscription page — you can see this is the higher-level container above your Resource Group

> 💡 **What this means:** The hierarchy in Azure is: **Tenant → Subscription → Resource Group → Resources**. Your Resource Group sits inside a Subscription, which is the billing and access boundary.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Overview tab with the Subscription name link highlighted and an arrow pointing to it]**

---

### Step 12: Navigate Back to Your Resource Group

1. Click the **Back button** in your browser, OR
2. Click **Resource Groups** in the breadcrumb trail at the top of the page
3. Click your Resource Group name again to return to it

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot showing the breadcrumb navigation at the top of the page with Resource Group link highlighted]**

---

## Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
□ Successfully logged in to the Azure Portal
□ Navigated to Resource Groups using the search bar
□ Found and opened your assigned Resource Group
□ Noted the Subscription name, Region, and resource count
□ Identified the Linux VM and its supporting resources in the list
□ Opened the Tags section and read the assigned tags
□ Understood the Azure hierarchy (Tenant → Subscription → RG → Resources)
```

---