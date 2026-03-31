# Task 2: Check IAM / RBAC on Your Resource Group

**Difficulty:** Beginner  
**Estimated Time:** 15–20 Minutes  
**Type:** Read Only (No changes will be made)

---

##  Objective

In this task, you will explore the **Access Control (IAM)** section of your Resource Group. You will learn what RBAC (Role-Based Access Control) is, how to check what permissions you have, and how to see that your Service Principal is also assigned a role on the Resource Group. 

---

##  Pre-Requisites

Before starting this task, make sure you have the following ready from your **Lab Details Page**:

| Item | Where to Find It |
|---|---|
| Azure Portal Login URL | Lab Details Page |
| Username | Lab Environment Page |
| Password | Lab Environment Page |
| Your Service Principal Name or Client ID | Lab Environment Page |

---

##  Quick Background: What is RBAC?

**RBAC (Role-Based Access Control)** is the system Azure uses to control **who can do what** on which resources.

- Every person or application that needs to interact with Azure must be **assigned a role**
- Roles are assigned at a specific **scope** (Tenant, Subscription, Resource Group, or individual resource)
- The three most common built-in roles are:

| Role | What It Can Do |
|---|---|
| **Owner** | Full control — can manage resources AND assign roles to others |
| **Contributor** | Can create and manage resources — cannot assign roles to others |
| **Reader** | Can only view resources — cannot make any changes |

---

##  Step-by-Step Instructions

---

### Step 1: Log In to the Azure Portal

1. Open your web browser
2. Go to:

```
https://portal.azure.com
```

3. Enter your **Username** from the Lab Details Page and click **Next**
4. Enter your **Password** and click **Sign in**
5. If prompted with **"Stay signed in?"**, click **No**

---
<img src="./images/Screenshot 2026-03-30 212318.png">
---

### Step 2: Navigate to Resource Groups

1. After logging in, you will be on the Azure Home Page
2. Click on the **Search Bar** at the top of the page
3. Type: `Resource Groups`
4. Click on **Resource Groups** from the dropdown results

---

<img src="./images/Screenshot 2026-03-30 212427.png">

---

### Step 3: Open Your Resource Group

1. In the Resource Groups list, locate your resource group
   - Example: `ODL-azure-Contoso-2157974`
2. Click on your **Resource Group name** to open it

---

<img src="./images/Screenshot 2026-03-30 214913.png">

---

### Step 4: Find the Access Control (IAM) Option

1. You are now inside your Resource Group on the **Overview** page
2. Look at the **left-hand side menu** (also called the left navigation pane)
3. Scroll down slightly in that left menu
4. Look for and click on **Access Control (IAM)**

>  **Tip:** IAM stands for **Identity and Access Management**. This is where all permission and role assignments are managed in Azure.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Resource Group left menu with "Access Control (IAM)" highlighted and an arrow pointing to it]**

---

### Step 5: Explore the IAM Overview Page

1. After clicking **Access Control (IAM)**, you will see a page with several sections and buttons
2. Read the buttons/tabs available at the top of this page:
   - **Add** — used to add role assignments
   - **Check access** — used to check what access a specific user has
   - **Role assignments** — shows all current assignments
   - **Roles** — shows all available roles you can assign
3. Do not click any buttons yet — just observe the layout

---

<img src="./images/Screenshot 2026-03-30 215048.png">

---

### Step 6: Check Your Own Access Using "View My Access"

1. On the **Access Control (IAM)** page, look for the section that says **"View my access"** or **"Check access"**
2. Click on the **"View my access"** button or **"Check access"** tab
3. A panel will open on the right side of the screen
4. It will show **your username** and what roles you currently have on this Resource Group

>  **What you will see:** You will see that your account has a role like **Contributor** or **Reader** assigned at the **Resource Group** scope. This means your access is limited to only this Resource Group — you cannot access other Resource Groups or the full Subscription.

---

<img src="./images/Screenshot 2026-03-30 215140.png">

---

### Step 7: Close the Panel and Go to Role Assignments Tab

1. Close the side panel by clicking the **X** at the top right of the panel
2. Now click on the **Role assignments** tab at the top of the IAM page

---

<img src="./images/Screenshot 2026-03-30 215309.png">

---

### Step 8: Identify Your Own User in the List

1. In the Role assignments list, look for your own **username**
2. Check what **Role** is assigned to your account
3. Check the **Scope** column — it should say **"This resource"** meaning it is scoped to this Resource Group only

> 💡 **What this means:** Your user account only has access to this specific Resource Group. If you were to try navigating to another Resource Group in this subscription, you would see an access denied or empty result — because your role does not extend there.

---

<img src="./images/Screenshot 2026-03-30 215620.png">

---

### Step 9: Understand the Difference Between Your User and the Service Principal

After looking at both entries, understand these key differences:

| | Your User Account | Service Principal |
|---|---|---|
| **What it is** | A human identity (you) | A non-human/app identity |
| **How it logs in** | Username + Password | Client ID + Client Secret |
| **Used by** | You in the Azure Portal | Azure DevOps pipelines |
| **Visible in** | Azure AD Users list | Azure AD App Registrations |
| **Role on this RG** | Contributor or Reader | Contributor |

---

---

### Step 10: Explore the Available Roles (No Assignment)

1. Click on the **Roles** tab on the IAM page
2. A list of all built-in Azure roles will appear — scroll through the list
3. Find and read the descriptions for:
   - **Owner**
   - **Contributor**
   - **Reader**
4. Click on **Contributor** role to open its details
5. Inside the Contributor details, you will see what **Actions** (permissions) it includes and what it **does not allow**
6. Click the **X** to close the role details panel

>  **What this means:** Azure has over 70 built-in roles. Understanding what each role allows helps you grant the **minimum required access** — a principle called **Least Privilege**. In real projects, you never assign Owner to everyone — you give the minimum role needed.

---

<img src="./images/Screenshot 2026-03-30 220056.png">
---

### Step 13: Return to the Overview Page

1. In the left menu, click **Overview** to return to the main Resource Group page
2. Confirm that everything looks exactly the same as when you started
3. No changes have been made

---

##  Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
□ Logged in to the Azure Portal successfully
□ Navigated to your Resource Group
□ Opened the Access Control (IAM) page
□ Used "View my access" to check your own role
□ Opened the Role Assignments tab and read the full list
□ Identified your own user account and its role in the list
□ Identified the Service Principal and its role in the list
□ Understood the difference between a User and a Service Principal
□ Explored the Roles tab and read the Contributor role details
□ Returned to Overview — confirmed no changes were made
```

---
