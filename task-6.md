# Task 6: Create a Service Connection in Azure DevOps

**Difficulty:** Beginner–Intermediate  
**Estimated Time:** 20–25 Minutes  
**Type:** Configuration (Creates a Service Connection in your DevOps project — does not affect Azure resources)

---

##  Objective

In this task, you will log in to **Azure DevOps**, set up a project, and create a **Service Connection** that links Azure DevOps to your Azure subscription using the Service Principal credentials from your Lab Details Page. You will then **verify the connection** to confirm authentication is working correctly.

---

##  Pre-Requisites

Before starting, make sure you have completed:
-  Task 4: Locate Your Service Principal Details

Have the following ready from your **Lab Details Page**:

| Item | Where to Find It |
|---|---|
| Azure DevOps Organization URL | Lab Environment Page (e.g., `https://dev.azure.com/contoso-lab`) |
| Azure DevOps Username | Lab Environment Page |
| Azure DevOps Password | Lab Environment Page |
| **Tenant ID** | Lab Environment Page |
| **Subscription ID** | Lab Environment Page |
| **Subscription Name** | Lab Environment Page |
| **Client ID (Application ID)** | Lab Environment Page |
| **Client Secret** | Lab Environment Page |

>  Keep your Lab Details Page open in a separate browser tab — you will be copying values from it during this task.

---

##  Quick Background: What Is a Service Connection?

A **Service Connection** in Azure DevOps is a **saved, named credential** that allows your pipelines to authenticate to external services — in this case, Azure.

**Without a Service Connection:**
- Every pipeline would need raw credentials (Client ID, Secret) hardcoded inside it — which is insecure

**With a Service Connection:**
- You save the credentials once, give the connection a name (e.g., `azure-sp-connection`)
- Pipelines reference it by name — credentials are never exposed in the pipeline code
- Azure DevOps **encrypts and stores** the credentials — even you cannot read them back after saving

---

##  Part A: Log In to Azure DevOps

---

### Step 1: Open Azure DevOps

1. Open a **new browser tab**
2. In the address bar, type the **Azure DevOps Organization URL** from your Lab Details Page
3. Press **Enter**

---

<img src="./images/Screenshot 2026-03-31 090504.png">

---

### Step 2: Sign In to Azure DevOps

1. The Azure DevOps sign-in page will appear
2. Enter your **Azure DevOps Username** from the Lab Details Page
3. Click **Next**
4. Enter your **Azure DevOps Password**
5. Click **Sign in**
6. If prompted to **"Stay signed in?"**, click **No**

---

<img src="./images/Screenshot 2026-03-31 090612.png">

---

### Step 3: Recognize the Azure DevOps Home Page

1. You will land on the **Azure DevOps Organization** home page
2. You will see one or more **Projects** listed
3. You should see a project pre-created for you (e.g., `contoso-lab-john`)
4. Click on your **Project name** to open it

---

<img src="./images/Screenshot 2026-03-31 090916.png">

---

### Step 4: Explore the Azure DevOps Project Page

1. After clicking your project, you will see the **Project Overview** page
2. On the **left sidebar**, you will see navigation links:
   - Boards
   - Repos
   - Pipelines
   - Test Plans
   - Artifacts
3. At the very **bottom of the left sidebar**, look for **Project Settings** (gear icon ⚙️)

---

<img src="./images/Screenshot 2026-03-31 091019.png">
---

##  Part B: Navigate to Service Connections

---

### Step 5: Open Project Settings

1. At the bottom-left of the screen, click on **Project Settings** (the gear icon ⚙️)
2. A new page titled **Project Settings** will open
3. This page manages all configurations for your project

---

<img src="./images/Screenshot 2026-03-31 091106.png">

---

### Step 6: Find Service Connections

1. On the **Project Settings** page, look at the left menu
2. Scroll down to find the **Pipelines** section in the left menu
3. Under **Pipelines**, click on **Service connections**

---

<img src="./images/Screenshot 2026-03-31 091149.png">

---

### Step 7: View the Service Connections Page

1. The **Service connections** page will open
2. It may say **"No service connections found"** if this is a fresh project
3. Look for the **"New service connection"** button (usually at the top right or center of the page)

---

<img src="./images/Screenshot 2026-03-31 091224.png">
---

##  Part C: Create the Service Connection

---

### Step 8: Click "New Service Connection"

1. Click the **New service connection** button
2. A panel or dialog will slide in from the right side of the screen
3. This shows a list of service connection types

---

<img src="./images/Screenshot 2026-03-31 091253.png">

---

### Step 9: Select "Azure Resource Manager"

1. In the list of service connection types, scroll and find **Azure Resource Manager**
2. Click on **Azure Resource Manager** to select it (it will be highlighted)
3. Click the **Next** button at the bottom of the panel

---

<img src="./images/Screenshot 2026-03-31 091328.png">

---

### Step 10: Select Authentication Method — "Service Principal (Manual)"

1. A new screen will appear asking **"How would you like to authenticate?"**
2. You will see several options:
   - App registration (automatic)
   - **App registration or managed identity (manual)**
   - Managed Identity
   - App Registration (manual)
   - Managed identity agent assigned
   - publish profile
  Select credential as 
   - Workload Identity federation (manual)
3. Select **App registration or managed identity (manual)**

4. Click Credentials as **Secret**

>  **Why manual?** We are entering the Service Principal details that were pre-created and provided to you. "Manual" means you will enter the credentials yourself rather than having Azure DevOps try to auto-detect them.

---

<img src="./images/Screenshot 2026-03-31 102449.png">

---

### Step 11: Fill In the Connection Details

1. A form will appear with several fields to fill in
2. Fill in each field using the values from your **Lab Environment Page**:

**Environment:**
- Select: **Azure Cloud**

**Tenant ID:**
- Provide the tenant id from the Environment page

click on next
  
**Scope Level:**
- Select: **Subscription**

**Subscription ID:**
- Copy the **Subscription ID** from your Lab Details Page and paste it here
- Example format: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

**Subscription:**
- Automatically taken by default.

**Application (client) ID**
- Provide the Client ID from the Environment tab
- Click on the security check box**

**Tenant ID**
- Provide the Value of the tenant from the Environment page.

**Secret Key**
- Provide the secret key from the environment page.
- Click on verify to verify the connection. Check for Verification Succeeded.
---

<img src="./images/Screenshot 2026-03-31 103050.png">

---

### Step 12: Name the Service Connection

1. Scroll down further on the form to find the **Service connection name** field
2. In this field, type exactly:

```
azure-sp-connection
```

3. Optionally, in the **Description** field, type:

```
Service connection using pre-provisioned Service Principal for lab exercises
```

---

### Step 13: Enable Access for All Pipelines

1. Look for a checkbox that says **"Grant access permission to all pipelines"**
2. Make sure this checkbox is **checked (enabled)**

>  **Why:** This allows any pipeline in your project to use this service connection without additional approval steps. In production, you might be more restrictive, but for this lab it should be enabled.

---

<img src="./images/Screenshot 2026-03-31 103254.png">

---

### Step 14: Save the Service Connection

1. After successful verification, click the **Save** button (or **"Verify and save"** button if combined)
2. Wait for the page to redirect

---

<img src="./images/Screenshot 2026-03-31 103254.png">

---

### Step 15: Confirm the Service Connection Was Created

1. After saving, you will be redirected back to the **Service connections** list page
2. You should now see your new connection listed:
   - **Name:** `azure-sp-connection`
   - **Type:** Azure Resource Manager
   - **Status:** Ready
3. Click on `azure-sp-connection` to open its details page and confirm all values

---

<img src="./images/Screenshot 2026-03-31 103602.png">

---

## Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
□ Logged in to Azure DevOps using credentials from the Lab Details Page
□ Opened your pre-created project
□ Navigated to Project Settings → Service connections
□ Clicked "New service connection"
□ Selected "Azure Resource Manager" as the type
□ Selected "Service principal (manual)" as the authentication method
□ Filled in Subscription ID and Subscription Name correctly
□ Filled in Service Principal Id (Client ID) correctly
□ Filled in Client Secret correctly
□ Filled in Tenant ID correctly
□ Named the connection "azure-sp-connection"
□ Enabled "Grant access permission to all pipelines"
□ Clicked Verify and confirmed the green success message
□ Clicked Save
□ Confirmed the connection appears in the Service Connections list with Ready status
□ Opened the connection details and confirmed the secret is not visible
```
---