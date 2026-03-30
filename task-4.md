# Task 4: Locate Your Service Principal Details

**Difficulty:** Beginner  
**Estimated Time:** 15–20 Minutes  
**Type:** Read Only (No changes will be made)

---

## Objective

In this task, you will use the details from your **Lab Details Page** to locate your **Service Principal** inside the Azure Portal. You will explore its properties, understand each component (Tenant ID, Client ID, Client Secret), and learn why the secret value is hidden in the portal.

---

## 📋 Pre-Requisites

Before starting, make sure you have the following ready from your **Lab Details Page**:

| Item | Where to Find It |
|---|---|
| Azure Portal Login URL | Lab Details Page |
| Username | Lab Details Page |
| Password | Lab Details Page |
| **Tenant ID** | Lab Details Page |
| **Client ID (Application ID)** | Lab Details Page |
| **Client Secret Value** | Lab Details Page |
| **Subscription ID** | Lab Details Page |

>  Keep your Lab Details Page open in a separate browser tab — you will need to refer back to it multiple times during this task.

---

##  Quick Background: What Is a Service Principal?

A **Service Principal** is like a **service account** or **non-human identity** in Azure.

- Humans log in with username and password
- Applications and pipelines log in using a **Service Principal** (Client ID + Client Secret)
- Your Azure DevOps pipeline uses this SP to authenticate to Azure and deploy resources

Think of it like this:

| | Human User | Service Principal |
|---|---|---|
| **Identity** | Your email address | Client ID (a GUID) |
| **Password** | Your portal password | Client Secret |
| **Visible in** | Azure AD Users | Azure AD App Registrations |
| **Used by** | You when browsing the portal | Pipelines, automation scripts |

---

##  Step-by-Step Instructions

---

### Step 1: Log In to the Azure Portal

1. Open your web browser
2. Go to:

```
https://portal.azure.com
```

3. Enter your **Username** from the Lab Details Page → click **Next**
4. Enter your **Password** → click **Sign in**
5. If prompted with **"Stay signed in?"**, click **No**
6. You should now be on the **Azure Portal Home Page**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Azure Portal home page after a successful login]**

---

### Step 2: Note Down Your SP Details from the Lab Details Page

Before doing anything in the portal, open your **Lab Details Page** and note down the following four values:

| Field | Your Value (Fill This In) |
|---|---|
| Tenant ID | |
| Subscription ID | |
| Client ID (Application ID) | |
| Client Secret | |

>  **Important:** Keep this information safe and do not share it with anyone. These are credentials that can be used to access your Azure environment.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of a sample Lab Details Page showing where Tenant ID, Subscription ID, Client ID, and Client Secret are located (values blurred for security)]**

---

### Step 3: Navigate to Microsoft Entra ID

1. Click on the **Search Bar** at the very top of the Azure Portal
2. Type: `Microsoft Entra ID`
3. In the dropdown results, click on **Microsoft Entra ID** (previously known as Azure Active Directory / Azure AD)

>  **Note:** You may also see it labeled as **Azure Active Directory** in some older portal views — they are the same thing. Microsoft renamed it to **Microsoft Entra ID** in 2023.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the search bar with "Microsoft Entra ID" typed and the result highlighted in the dropdown]**

---

### Step 4: Explore the Microsoft Entra ID Overview Page

1. The **Microsoft Entra ID** overview page will open
2. Look at the information visible here:
   - **Tenant Name** — the name of your Azure AD tenant
   - **Tenant ID** — a long GUID (unique identifier)
3. Compare the **Tenant ID** shown here with the **Tenant ID** from your Lab Details Page
4. They should match exactly

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Microsoft Entra ID Overview page with the Tenant ID field highlighted and labeled]**

---

### Step 5: Navigate to App Registrations

1. In the **left-hand menu** of the Microsoft Entra ID page, scroll down
2. Look for a section called **Manage**
3. Under **Manage**, click on **App registrations**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Entra ID left menu with the "Manage" section visible and "App registrations" highlighted]**

---

### Step 6: Switch to "All Applications" Tab

1. The **App registrations** page will open
2. At the top, you will see tabs:
   - **Owned applications**
   - **All applications**
   - **Deleted applications**
3. Click on the **All applications** tab
4. A list of all Service Principals (App Registrations) will appear

>  **Why:** Your SP was created by the lab automation system — not by your user account. So it may not appear under "Owned applications" and you need to look under "All applications."

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the App Registrations page with the "All applications" tab selected and a list of applications visible]**

---

### Step 7: Search for Your Service Principal Using the Client ID

1. On the **All applications** tab, you will see a **Search box** at the top of the list
2. From your Lab Details Page, copy your **Client ID**
   - It will look like this format: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`
3. Paste or type the **Client ID** into the search box
4. The list will filter down to show only your Service Principal
5. Click on your **Service Principal name** to open it

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the All Applications search box with the Client ID pasted in and one matching result visible in the list]**

---

### Step 8: Explore the Service Principal Overview Page

1. The **App Registration overview** page for your Service Principal will open
2. Look at the fields on this page and match them with your Lab Details Page:

| Field on Portal | What It Is | Should Match |
|---|---|---|
| **Display name** | Friendly name of the SP | SP Name on lab page |
| **Application (client) ID** | The Client ID | Client ID on lab page |
| **Directory (tenant) ID** | The Tenant ID | Tenant ID on lab page |
| **Object ID** | Internal Azure ID | (no need to match) |

3. Confirm the **Application (client) ID** and **Directory (tenant) ID** match your lab page values exactly

---

>  **[IMAGE PLACEHOLDER — Screenshot of the App Registration Overview page with Application (client) ID and Directory (tenant) ID fields highlighted and labeled]**

---

### Step 9: Explore the Certificates & Secrets Section

1. In the **left menu** of the App Registration page, look under **Manage**
2. Click on **Certificates & secrets**
3. The **Certificates & secrets** page will open
4. You will see two tabs:
   - **Client secrets**
   - **Certificates**
5. Make sure you are on the **Client secrets** tab

---

>  **[IMAGE PLACEHOLDER — Screenshot of the Certificates & secrets page with the "Client secrets" tab selected]**

---

### Step 10: Observe the Secret Entry (Value Is Hidden)

1. You will see an entry in the **Client secrets** list
2. Look at the columns available:
   - **Description** — a label for the secret
   - **Expires** — when this secret will expire
   - **Value** — you will see only **`Hidden value`** here — the actual secret is NOT shown
   - **Secret ID** — an internal identifier for the secret
3. Notice that the **Value column shows "Hidden value"** and there is no way to reveal it from here

>  **Why is the value hidden?**
> Azure only shows you a secret's value **at the moment it is created**. After that, Azure stores it encrypted and **never shows it again**. This is a security design — even Azure administrators cannot retrieve it after creation.
> This is exactly why your Lab Details Page shows the Client Secret — it was captured by the automation script **at the moment the secret was generated**.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Client secrets list showing a secret with "Hidden value" in the Value column, with that column highlighted and labeled]**

---

### Step 11: Verify the Secret Expiry Date

1. On the same **Client secrets** tab, look at the **Expires** column
2. Note down the expiry date of the secret
3. In a real project, when a secret expires, pipelines using this SP will **fail to authenticate** until the secret is rotated

>  **Best Practice:** In production, always set up alerts to notify your team before a Service Principal secret expires. Many teams use a 6-month or 1-year expiry and rotate secrets as part of their security practices.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Client secrets list with the Expires column highlighted showing the expiry date]**

---

### Step 12: Navigate to API Permissions (Explore Only)

1. In the left menu, click on **API permissions**
2. This page shows what Microsoft APIs this Service Principal has been granted access to
3. You may see **Microsoft Graph** or **Azure Service Management** listed
4. Do not add or remove any permissions — just read and observe

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the API permissions page showing the list of permissions granted to the Service Principal]**

---

### Step 13: Answer These Reflection Questions

Before completing this task, answer the following questions in your own words:

1. **Why can't you see the Client Secret value in the Certificates & Secrets page?**
   
   *Your Answer:*

2. **What is the difference between the Client ID and the Client Secret?**
   
   *Your Answer:*

3. **What would happen if the Client Secret expires and you don't rotate it?**
   
   *Your Answer:*

---

### Step 14: Return to the Azure Home Page

1. In the top-left area, click the **Microsoft Azure** logo or the hamburger menu (≡)
2. This will take you back to the Azure Portal home page
3. Confirm you are back on the home page without having made any changes

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Azure home page after navigating back, confirming no changes were made]**

---

##  Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
□ Noted all four SP values from the Lab Details Page (Tenant ID, Subscription ID, Client ID, Secret)
□ Navigated to Microsoft Entra ID and confirmed the Tenant ID matches
□ Opened App Registrations → All Applications tab
□ Searched using the Client ID and found your Service Principal
□ Opened the SP overview page and confirmed Client ID and Tenant ID match lab page
□ Opened Certificates & Secrets → Client secrets tab
□ Observed that the secret value is "Hidden" and understood why
□ Noted the secret expiry date
□ Explored API permissions
□ Answered the three reflection questions
□ Returned to Azure Home — no changes made
```

---