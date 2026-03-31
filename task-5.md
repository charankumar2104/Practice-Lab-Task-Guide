# Task 5: Assign RBAC to the Service Principal at Subscription Level 

**Difficulty:** Intermediate  
**Estimated Time:** 25-30 Minutes  
**Type:** Hands-On Change with Full Revert (You will assign a role to the Service Principal at Subscription scope and then remove it to restore the original state)

---

## Objective

In this task, you will assign the **Reader** role to your pre-provisioned Service Principal at the **Subscription** level using the Azure Portal. This is different from the RBAC you explored in Task 3, where the Service Principal had Contributor access only at the Resource Group level. Assigning a role at the Subscription level means the Service Principal will be able to contribute resources across all Resource Groups within the Subscription — not just your own. You will verify the assignment, understand the difference between Subscription-level and Resource Group-level scope

---

## Pre-Requisites

Before starting this task, make sure you have completed:
- Task 3: Check IAM and RBAC on Your Resource Group
- Task 4: Assign a Reader Role and Remove It
- Task 7: Locate Your Service Principal Details

Have the following ready from your Lab Details Page:

| Item | Where to Find It |
|---|---|
| Azure Portal Login URL | Lab Environment Page |
| Username | Lab Environment Page |
| Password | Lab Environment Page |
| Client ID (Application ID) | Lab Environment Page — used to identify the Service Principal |
| Subscription Name | Lab Environment Page |
| Subscription ID | Lab Environment Page |

---

## Quick Background: Subscription-Level RBAC vs Resource Group-Level RBAC

In Task 3, you saw that your Service Principal already has the **Contributor** role at the **Resource Group** level. This means the Service Principal can create, modify, and delete resources inside your specific Resource Group — but it cannot see or touch anything outside of it.

When you assign a role at the **Subscription** level, the scope of that permission becomes much wider:

| Scope | What the SP Can Access |
|---|---|
| Resource Group level | Only the resources inside that specific Resource Group |
| Subscription level | All Resource Groups and all resources across the entire Subscription |

This is a significant difference. In real organizations, Subscription-level access is treated with much more caution because it gives visibility or control over everything inside the Subscription — including Resource Groups belonging to other teams.

The diagram below shows the scope difference:

```
Azure Subscription
    |
    |-- Resource Group A  (SP with Subscription-level role can see this)
    |       |-- VM-A
    |       |-- Storage Account-A
    |
    |-- Resource Group B  (SP with Subscription-level role can see this)
    |       |-- VM-B
    |
    |-- rg-participant-yourname  (SP already has Contributor here via RG-level role)
            |-- Your Linux VM
```

In this task you will assign the **Reader** role at the Subscription level. Reader is chosen deliberately because it is the lowest-privilege role — the SP can only view resources across the Subscription, not create or modify anything.

---

## Part A: Note the Current Subscription-Level Role Assignments

---

### Step 1: Log In to the Azure Portal

1. Open your web browser
2. Go to:

```
https://portal.azure.com
```

3. Enter your Username from the Lab Details Page and click Next
4. Enter your Password and click Sign in
5. If prompted with "Stay signed in?", click No
6. You will land on the Azure Portal home page

---

<img src="./images/Screenshot 2026-03-30 212318.png">

---

### Step 2: Navigate to the Subscriptions Page

1. Click the Search Bar at the top of the Azure Portal
2. Type: Subscriptions
3. In the dropdown results, click on **Subscriptions**
4. The Subscriptions page will open showing a list of subscriptions you have access to

---

<img src="./images/Screenshot 2026-03-31 084011.png">

---

### Step 3: Open Your Subscription

1. In the Subscriptions list, find your subscription using the Subscription Name from your Lab Details Page
2. Click on your **Subscription name** to open it
3. The Subscription overview page will open

---

<img src="./images/Screenshot 2026-03-31 084056.png">
---

### Step 4: Explore the Subscription Overview Page

1. On the Subscription overview page, look at the information displayed:
   - **Subscription ID** — verify this matches the Subscription ID on your Lab Details Page
   - **Directory** — this is your Entra ID tenant name
   - **Status** — should show Active
2. Note that this page is at a higher level than the Resource Group — you can see all Resource Groups in this Subscription from here

---

<img src="./images/Screenshot 2026-03-31 084113.png">
---

### Step 5: Open Access Control (IAM) at the Subscription Level

1. In the left-hand menu of the Subscription page, look for **Access Control (IAM)**
2. Click on **Access Control (IAM)**
3. The IAM page for the Subscription will open

> Note: This is the same IAM interface you used in Tasks 3 and 4, but now you are at the Subscription level — not the Resource Group level. Any role you assign here will apply across the entire Subscription.

---

<img src="./images/Screenshot 2026-03-31 084233.png">

---

### Step 6: View the Current Role Assignments at Subscription Level

1. Click the **Role assignments** tab
2. Read through the list of all role assignments at the Subscription level
3. Look for your Service Principal in this list using the Client ID or SP name from your Lab Details Page
4. At this point, your Service Principal should **not** appear in this list — it currently only has a role at the Resource Group level, not the Subscription level
5. Note down the total number of role assignment entries currently shown in the list — you will use this to verify the revert at the end of the task

---

<img src="./images/Screenshot 2026-03-31 084328.png">

---

## Part B: Assign the Reader Role to the Service Principal at Subscription Level

---

### Step 7: Click Add and Select Add Role Assignment

1. At the top of the Access Control (IAM) page, click the **+ Add** button
2. A dropdown will appear with two options:
   - Add role assignment
   - Add co-administrator
3. Click **Add role assignment**
4. The Add role assignment page will open

---

<img src="./images/Screenshot 2026-03-31 084411.png">

---

### Step 8: Search for and Select the Reader Role

1. You are on the **Role** step of the Add role assignment wizard
2. The page shows a list of roles — there are many roles available
3. In the search box above the role list, type: Reader
4. The list will filter down to show roles with "Contributor" in the name
5. Find the role named exactly **Contributor** (the description will say "View all resources, but does not allow you to make any changes")
6. Click on **Contributor** to select it — a blue checkmark or highlight will appear next to it
7. Click the **Next** button at the bottom of the page

---

<img src="./images/Screenshot 2026-03-31 085143.png">

---

### Step 9: Navigate to the Members Step

1. You are now on the **Members** step
2. Under "Assign access to", make sure **User, group, or service principal** is selected
3. Click the **+ Select members** link
4. A search panel will open on the right side of the screen

---

<img src="./images/Screenshot 2026-03-31 085351.png">

---

### Step 10: Search for Your Service Principal

1. In the search panel that opened on the right, you will see a search box
2. From your Lab Details Page, take note of your Client ID (Application ID)
3. Type the **name of your Service Principal** or the first few characters of the Client ID into the search box
   - If you know the display name of your SP (visible on the Lab Details Page as Display name), type that name
   - Example: if your SP is named  `https://odl_user_sp_2157974`, type `https://odl_user_sp_2157974`
4. Your Service Principal will appear in the search results below the search box
5. The result will show a type indicator showing it is an Application or Service Principal — not a regular user
6. Click on your **Service Principal** to select it — a checkmark will appear next to it
7. Click the **Select** button at the bottom of the panel

---

><img src="./images/Screenshot 2026-03-31 085409.png">

---

### Step 11: Confirm the Service Principal Is Listed Under Members

1. After clicking Select, the panel will close
2. You will see your Service Principal now listed under the Members section
3. Confirm the entry shows:
   - The correct Service Principal name
   - Object type: Service Principal or Application
4. Click the **Next** button to proceed to the Review step

---

> [IMAGE PLACEHOLDER - Screenshot of the Members step with the Service Principal listed under Members showing its name and type before clicking Next]

---

### Step 12: Review the Assignment on the Review + Assign Tab

1. You are now on the **Review + assign** step
2. Review the summary carefully and confirm all three details are correct:
   - **Role:** Reader
   - **Scope:** Your Subscription name (this confirms the role is being assigned at Subscription level, not RG level)
   - **Members:** Your Service Principal name
3. Confirm the scope shows the Subscription — not a Resource Group name
4. Click the **Review + assign** button at the bottom of the page

---

<img src="./images/Screenshot 2026-03-31 085618.png">

---

### Step 13: Confirm the Assignment Was Successful

1. After clicking Review + assign, you will be returned to the Access Control (IAM) page
2. A green notification banner will appear at the top right saying the role assignment was added successfully
3. Click on the **Role assignments** tab
4. Look through the list for your Service Principal
5. You should now see your Service Principal listed with the **Contributor** role and the scope showing **This resource** — which at the Subscription level means the entire Subscription

---

<img src="./images/Screenshot 2026-03-31 085856.png">

---

### Step 14: Understand What the Assignment Means

Now that the Reader role has been assigned to the Service Principal at the Subscription level, the SP has two role assignments in total:

| Role | Scope | What It Allows |
|---|---|---|
| Contributor | Your Resource Group (rg-participant-yourname) | Create, modify, and delete resources inside your specific RG |
| Reader | Subscription | View all resources and Resource Groups across the entire Subscription |

The SP's effective permissions are now a combination of both:
- Inside your Resource Group: Contributor (higher role takes precedence at that scope)
- In all other Resource Groups in the Subscription: Reader only — it can view but not change anything

---

### Step 15: Verify the Subscription-Level Role by Navigating to Another Resource Group

1. In the left menu, click on **Resource Groups** or use the search bar to go to Resource Groups
2. You will see all Resource Groups in the Subscription
3. Click on a Resource Group that is **not** your own lab Resource Group
4. Inside that Resource Group, click on **Access Control (IAM)** from the left menu
5. Click **Role assignments** tab
6. Look for your Service Principal in this list
7. You should see your SP listed here with the **Reader** role, inherited from the Subscription level

> Note: If you do not see the SP here, it is because the inherited role may take a moment to propagate. You can also use the "Check access" option and search for the SP to see its effective permissions.

> This confirms that the Subscription-level role assignment gives the SP visibility across the entire Subscription, including Resource Groups it was never explicitly added to.

---

## Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
[ ] Logged in to the Azure Portal
[ ] Navigated to the Subscriptions page and opened your Subscription
[ ] Opened Access Control (IAM) at the Subscription level
[ ] Noted the current Role Assignments count before making any changes
[ ] Confirmed the Service Principal was not listed at Subscription level before the task
[ ] Clicked Add and selected Add role assignment
[ ] Selected the Reader role from the role list
[ ] Searched for and selected the Service Principal as the member
[ ] Reviewed the assignment (Role = Reader, Scope = Subscription, Member = SP)
[ ] Clicked Review + assign and confirmed the green success notification
[ ] Verified the SP appeared in the Subscription-level Role Assignments list
[ ] Navigated to another Resource Group and confirmed the inherited Reader role was visible there
```

