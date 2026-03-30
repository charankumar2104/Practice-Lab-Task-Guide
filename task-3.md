# Task 3: Assign a Reader Role → Then Remove It (Revert to Default)

**Difficulty:** Beginner–Intermediate  
**Estimated Time:** 20–25 Minutes  
**Type:** Hands-On Change + Revert (You will make a change and then undo it completely)

---

##  Objective

In this task, you will **manually assign a Reader role** to your own user account on your Resource Group, observe the effect, and then **remove it** to restore everything back to the original state. This gives you real hands-on experience with Azure RBAC role assignment and removal.

---

##  Pre-Requisites

Before starting this task, make sure you have completed:
-  Task 1: Explore Your Resource Group
-  Task 2: Check IAM / RBAC (so you know your current role before making changes)

Also have ready from your **Lab Details Page**:

| Item | Where to Find It |
|---|---|
| Azure Portal Login URL | Lab Details Page |
| Username | Lab Details Page |
| Password | Lab Details Page |
| Your Resource Group Name | Lab Details Page |

---

##  Quick Background: What Will You Be Doing?

You will assign the **Reader** role to your own user account at the **Resource Group scope**.

- After assigning, your user will have **two role entries** — your original role + Reader
- Azure RBAC is **additive** — having both Contributor and Reader means the higher role (Contributor) takes effect
- You will then **remove** the Reader role so the assignment list goes back to exactly what it was before

>  **Important:** You are only adding/removing the **Reader** role. Your original role will not be touched at any point during this task.

---

##  Part A: Assign the Reader Role

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

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Azure Portal login page]**

---

### Step 2: Navigate to Your Resource Group

1. Click the **Search Bar** at the top of the Azure Portal home page
2. Type: `Resource Groups`
3. Click **Resource Groups** from the dropdown
4. Find and click on your Resource Group (e.g., `rg-participant-john`)

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot showing the search for Resource Groups and the list with the participant's RG highlighted]**

---

### Step 3: Open Access Control (IAM)

1. Inside your Resource Group, look at the **left-hand menu**
2. Scroll down and click on **Access Control (IAM)**
3. The IAM page will open

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the left menu inside the Resource Group with "Access Control (IAM)" highlighted]**

---

### Step 4: Take a Note of the Current Role Assignments

1. Click the **Role assignments** tab
2. Look at the current list of assignments
3. Note down **how many entries** are there and what your user's current role is

>  **Why this matters:** You need to remember the original state so you can confirm you have properly reverted at the end of this task.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Role Assignments tab showing the current list before any changes, with the participant's current role visible]**

---

### Step 5: Click the "Add" Button

1. At the top of the **Access Control (IAM)** page, click the **+ Add** button
2. A small dropdown menu will appear with two options:
   - **Add role assignment**
   - **Add co-administrator**
3. Click **Add role assignment**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the IAM page with the "+ Add" button clicked and the dropdown showing "Add role assignment" highlighted]**

---

### Step 6: The "Add Role Assignment" Page Opens

1. A new page titled **"Add role assignment"** will open
2. You will see three steps at the top: **Role**, **Members**, **Review + assign**
3. You are currently on the **Role** step
4. In the search box under the roles list, type: `Reader`
5. The list will filter and show the **Reader** role
6. Click on **Reader** to select it — a blue checkmark or highlight will appear next to it
7. Click the **Next** button at the bottom

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the "Add role assignment" page with "Reader" searched, selected in the list, and the Next button highlighted]**

---

### Step 7: Select the Member (Your User Account)

1. You are now on the **Members** step
2. Make sure **"User, group, or service principal"** is selected under "Assign access to"
3. Click the **+ Select members** link/button
4. A panel will open on the right side — a search box will appear
5. Type your **username** or part of your name in the search box
   - Example: type `john` to find `john@contosolab.onmicrosoft.com`
6. Your user account will appear in the results below the search box
7. Click on your **username** to select it — a checkmark will appear next to it
8. Click the **Select** button at the bottom of the panel

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Members step with the Select members panel open, the participant's username searched, selected (checkmark visible), and the Select button highlighted]**

---

### Step 8: Confirm the Member Is Added

1. After clicking **Select**, the panel will close
2. You will see your username now listed under the **Members** section on the page
3. Confirm it shows:
   - **Name:** Your username
   - **Object type:** User
4. Click the **Next** button to proceed

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Members step showing the participant's username successfully added to the Members list before clicking Next]**

---

### Step 9: Review the Assignment on the "Review + Assign" Tab

1. You are now on the **Review + assign** step
2. This is a summary page — review it carefully:
   - **Role:** Reader
   - **Scope:** Your Resource Group name
   - **Members:** Your username
3. Confirm all three are correct
4. Click the **Review + assign** button at the bottom of the page

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the "Review + assign" tab showing Role = Reader, Scope = the RG name, and Member = participant username, with the button highlighted]**

---

### Step 10: Confirm the Assignment Was Successful

1. After clicking **Review + assign**, you will be taken back to the **Access Control (IAM)** page
2. A green notification banner will appear at the top right saying something like **"Role assignment added"** or **"Successfully added role assignment"**
3. Click on the **Role assignments** tab
4. You will now see **a new entry** in the list:
   - Your username appearing **twice** — once with your original role and once with **Reader**

>  **What just happened:** You successfully assigned the Reader role to yourself. Azure RBAC is additive — both role entries exist simultaneously. Your effective access is the **highest role** — Contributor if that was your original role.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Role Assignments tab showing the participant's username listed twice — once with original role and once with Reader, with the green success notification visible]**

---

##  Part B: Remove the Reader Role (Revert to Default)

---

### Step 11: Find the Reader Role Entry You Just Added

1. You should already be on the **Role assignments** tab of Access Control (IAM)
2. If not, click **Access Control (IAM)** in the left menu, then click **Role assignments** tab
3. Look through the list for your username with the **Reader** role
4. Be careful — your username appears twice. Make sure you identify the one where the **Role** column says **Reader**

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Role Assignments list with both entries for the participant username visible, and the Reader entry highlighted/circled for clarity]**

---

### Step 12: Select the Reader Role Entry for Removal

1. Find the row with your username and **Reader** role
2. On the far right of that row, look for a **checkbox** — click it to select that row
3. The row will become highlighted when selected

>  **Important:** Only check the box next to the **Reader** entry — do NOT check the box next to your original role entry.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot showing the Reader role row selected with the checkbox checked (other rows unchecked), highlighting only the Reader row is selected]**

---

### Step 13: Click Remove

1. With the Reader row selected, look at the top of the Role assignments list
2. You will see a **Remove** button appear (it may have been greyed out before you selected a row)
3. Click the **Remove** button

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot showing the Remove button at the top of the Role Assignments list now active/clickable, with an arrow pointing to it]**

---

### Step 14: Confirm the Removal

1. A confirmation dialog box will appear asking: **"Do you want to remove the following role assignment(s)?"**
2. Read the confirmation — it should show the **Reader** role for your username
3. Click **Yes** to confirm the removal

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the confirmation dialog showing the Reader role assignment details and the Yes button highlighted]**

---

### Step 15: Verify the Removal Was Successful

1. After clicking **Yes**, you will be returned to the **Role assignments** tab
2. A green notification banner should appear saying something like **"Role assignment removed"**
3. Look at the Role assignments list now
4. Your username should appear **only once** with your original role — the Reader entry is gone
5. The list should look **exactly the same** as it did in Step 4 (before you made any changes)

>  **What just happened:** The Reader role has been completely removed. Your access is now exactly what it was before this task started. RBAC changes in Azure are immediate — there is no delay.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the Role Assignments tab after removal, showing the participant's username only once with their original role, and the green success notification visible]**

---

### Step 16: Final Verification — Count the Entries

1. Count the total number of role assignment entries in the list now
2. Compare it to what you noted in **Step 4**
3. The count should be **identical** to before you started

>  If the count matches and you see your username only once with the original role, the revert is complete and this task is done.

---

> 📸 **[IMAGE PLACEHOLDER — Screenshot of the final Role Assignments list confirming the count matches the original and the participant username appears only once]**

---

##  Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
□ Logged in to the Azure Portal
□ Navigated to your Resource Group and opened Access Control (IAM)
□ Noted the original role assignments before making any changes
□ Successfully added the Reader role to your user account
□ Confirmed the Reader entry appeared in the Role Assignments list
□ Selected ONLY the Reader role entry for removal
□ Clicked Remove and confirmed with "Yes"
□ Verified the Reader entry is gone from the list
□ Confirmed your username now appears only once with the original role
□ Confirmed the Role Assignments list matches the original state from Step 4
```

---