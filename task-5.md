# Task 5: Create a Service Connection in Azure DevOps

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
   - Example: `https://dev.azure.com/contoso-lab`
3. Press **Enter**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of a browser with the Azure DevOps organization URL typed in the address bar]**

---

### Step 2: Sign In to Azure DevOps

1. The Azure DevOps sign-in page will appear
2. Enter your **Azure DevOps Username** from the Lab Details Page
3. Click **Next**
4. Enter your **Azure DevOps Password**
5. Click **Sign in**
6. If prompted to **"Stay signed in?"**, click **No**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Azure DevOps sign-in page with the username and password fields visible]**

---

### Step 3: Recognize the Azure DevOps Home Page

1. You will land on the **Azure DevOps Organization** home page
2. You will see one or more **Projects** listed
3. You should see a project pre-created for you (e.g., `contoso-lab-john`)
4. Click on your **Project name** to open it

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Azure DevOps Organization home page with the participant's project listed and highlighted]**

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

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Azure DevOps project page showing the left sidebar with all navigation links and the Project Settings gear icon at the bottom]**

---

##  Part B: Navigate to Service Connections

---

### Step 5: Open Project Settings

1. At the bottom-left of the screen, click on **Project Settings** (the gear icon ⚙️)
2. A new page titled **Project Settings** will open
3. This page manages all configurations for your project

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Project Settings page opening, with the gear icon highlighted at the bottom of the sidebar]**

---

### Step 6: Find Service Connections

1. On the **Project Settings** page, look at the left menu
2. Scroll down to find the **Pipelines** section in the left menu
3. Under **Pipelines**, click on **Service connections**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Project Settings left menu with "Service connections" highlighted under the Pipelines section]**

---

### Step 7: View the Service Connections Page

1. The **Service connections** page will open
2. It may say **"No service connections found"** if this is a fresh project
3. Look for the **"New service connection"** button (usually at the top right or center of the page)

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the empty Service Connections page with "No service connections found" message and the "New service connection" button highlighted]**

---

##  Part C: Create the Service Connection

---

### Step 8: Click "New Service Connection"

1. Click the **New service connection** button
2. A panel or dialog will slide in from the right side of the screen
3. This shows a list of service connection types

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the "New service connection" panel open on the right side showing a list of connection types]**

---

### Step 9: Select "Azure Resource Manager"

1. In the list of service connection types, scroll and find **Azure Resource Manager**
2. Click on **Azure Resource Manager** to select it (it will be highlighted)
3. Click the **Next** button at the bottom of the panel

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the service connection type list with "Azure Resource Manager" selected and highlighted, and the Next button visible]**

---

### Step 10: Select Authentication Method — "Service Principal (Manual)"

1. A new screen will appear asking **"How would you like to authenticate?"**
2. You will see several options:
   - App registration or managed identity (automatic)
   - App registration or managed identity (manual)
   - **Service principal (manual)**
   - Workload Identity federation (manual)
3. Select **Service principal (manual)**
4. Click the **Next** button

>  **Why manual?** We are entering the Service Principal details that were pre-created and provided to you. "Manual" means you will enter the credentials yourself rather than having Azure DevOps try to auto-detect them.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the authentication method selection with "Service principal (manual)" selected and the Next button highlighted]**

---

### Step 11: Fill In the Connection Details

1. A form will appear with several fields to fill in
2. Fill in each field using the values from your **Lab Details Page**:

**Environment:**
- Select: **Azure Cloud**
  
**Scope Level:**
- Select: **Subscription**

**Subscription ID:**
- Copy the **Subscription ID** from your Lab Details Page and paste it here
- Example format: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

**Subscription Name:**
- Type the **Subscription Name** from your Lab Details Page
- Example: `Contoso-Training-Subscription`

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the service connection form showing Environment = Azure Cloud, Scope Level = Subscription, and the Subscription ID and Name fields filled in]**

---

### Step 12: Fill In the Service Principal Credentials

Continue filling in the form below the subscription fields:

**Service Principal Id:**
- Copy the **Client ID** from your Lab Details Page and paste it here
- This is also called **Application (client) ID**

**Service Principal key (Client Secret):**
- Copy the **Client Secret** from your Lab Details Page and paste it here
- Be careful to copy the exact value — do not include any extra spaces

**Tenant ID:**
- Copy the **Tenant ID** from your Lab Details Page and paste it here

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the lower portion of the form with Service Principal Id, Client Secret, and Tenant ID fields filled in (values partially blurred for security)]**

---

### Step 13: Name the Service Connection

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

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the service connection name field filled in with "azure-sp-connection" and the optional description field]**

---

### Step 14: Enable Access for All Pipelines

1. Look for a checkbox that says **"Grant access permission to all pipelines"**
2. Make sure this checkbox is **checked (enabled)**

>  **Why:** This allows any pipeline in your project to use this service connection without additional approval steps. In production, you might be more restrictive, but for this lab it should be enabled.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot showing the "Grant access permission to all pipelines" checkbox checked/enabled]**

---

### Step 15: Click "Verify" to Test the Connection

1. Before saving, click the **Verify** button (it may be labeled **"Verify and save"** or appear as a separate **Verify** link/button near the top of the form)
2. Wait a few seconds while Azure DevOps tests the credentials

**What happens during Verify:**
- Azure DevOps takes your Client ID, Client Secret, and Tenant ID
- It calls the Microsoft Azure authentication endpoint
- It checks if the credentials are valid and if the SP has access to the Subscription ID you provided
- If all checks pass — **green success message**
- If credentials are wrong — **red error message**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot showing the Verify button being clicked and a loading/spinner state while verification is in progress]**

---

### Step 16: Confirm Verification Is Successful

1. After verification completes, look for a **green checkmark** or a message that says **"Verification succeeded"** or **"Successfully verified"**
2. This confirms that Azure DevOps can authenticate to your Azure subscription using the Service Principal

> **If Verification Fails:**
> - Double-check that you copy-pasted all values correctly (no extra spaces)
> - Make sure the Client Secret is exactly as shown on the Lab Details Page
> - Check the Tenant ID and Subscription ID are correct
> - Contact your instructor if it still fails after checking

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot showing the green verification success message/checkmark near the Verify button or at the top of the form]**

---

### Step 17: Save the Service Connection

1. After successful verification, click the **Save** button (or **"Verify and save"** button if combined)
2. Wait for the page to redirect

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Save or "Verify and save" button highlighted at the bottom of the form]**

---

### Step 18: Confirm the Service Connection Was Created

1. After saving, you will be redirected back to the **Service connections** list page
2. You should now see your new connection listed:
   - **Name:** `azure-sp-connection`
   - **Type:** Azure Resource Manager
   - **Status:** Ready
3. Click on `azure-sp-connection` to open its details page and confirm all values

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Service Connections list showing "azure-sp-connection" listed with Type = Azure Resource Manager and a Ready status]**

---

### Step 19: Explore the Service Connection Details Page

1. After clicking on `azure-sp-connection`, you will see its details
2. Notice that the **Client Secret is NOT visible** — it has been encrypted and stored by Azure DevOps
3. You can see:
   - Connection name
   - Subscription name and ID
   - Service principal (Client) ID
   - Tenant ID
   - But NOT the Client Secret value

> **What this means:** Once saved, even you cannot retrieve the Client Secret from Azure DevOps. This is the same security principle as Azure's Certificates & Secrets page — secrets are write-once, never read-back.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the azure-sp-connection detail page showing the fields visible and confirming Client Secret is not shown]**

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