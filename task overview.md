# Task Overview: Azure DevOps with Azure Infrastructure

**Lab Title:** Azure DevOps with Azure Infrastructure - End-to-End Hands-On Lab  
**Client:** Contoso Innovations Inc.  
**Total Tasks:** 11  
**Total Estimated Duration:** 4 to 5 Hours

---

## How to Use This Document

This document gives you a complete picture of all the tasks you will perform in this lab, listed in the order you should complete them. Read the overview of each task before you begin it so you know what to expect and what the end goal is.

Each task has its own detailed step-by-step guide document. Once you finish reading the overview of a task here, open the corresponding task guide document and follow the steps one by one.

Do not skip tasks or change the order. Each task builds on the knowledge and setup from the previous one.

---

## Task List at a Glance

| Task Number | Task Name | Type | Estimated Time |
|---|---|---|---|
| Task 1 | Explore Your Pre-Provisioned Resource Group | Read Only | 15-20 mins |
| Task 2 | Check IAM and RBAC on Your Resource Group | Read Only | 15-20 mins |
| Task 3 | Assign a Reader Role and Remove It | Hands-On with Revert | 20-25 mins |
| Task 4 | Locate Your Service Principal Details | Read Only | 15-20 mins |
| Task 5 | Assign RBAC to the Service Principal at Subscription Level  | Hands-On with Revert | 25-30 mins |
| Task 6 | Create a Service Connection in Azure DevOps | Configuration | 20-25 mins |
| Task 7 | Run a Pipeline Using the Service Connection | Hands-On Pipeline | 25-30 mins |
| Task 8 | Deploy Azure Resources from ARM Template via Pipeline and Clean Up | Hands-On with Revert | 45-50 mins |

---

## Task 1: Explore Your Pre-Provisioned Resource Group

**Type:** Read Only  
**Estimated Time:** 15-20 Minutes  
**Detailed Guide:** Task-1_Explore_Resource_Group.md

### What You Will Do

In this task, you will log in to the Azure Portal for the first time using the credentials provided on your Lab Details Page. You will navigate to the Resource Groups section, find your assigned Resource Group, and explore everything inside it. You will read the tags that have been applied to your Resource Group and understand the Azure hierarchy — how Tenant, Subscription, Resource Group, and individual Resources relate to each other.

### What You Will See

When you open your Resource Group, you will find several resources inside it including a Linux Virtual Machine and its supporting infrastructure — a Network Interface, Virtual Network, OS Disk, Public IP Address, and Network Security Group. You will also see tags such as Environment, Owner, and CostCenter that were applied when the Resource Group was created.

### What You Will Learn

- How to navigate the Azure Portal using the search bar
- What a Resource Group is and what it contains
- What tags are and why organizations use them
- The Azure resource hierarchy from Tenant down to individual resources
- The difference between a Resource Group and a Subscription

### Changes Made

None. This task is entirely read-only.

---

## Task 2: Check IAM and RBAC on Your Resource Group

**Type:** Read Only  
**Estimated Time:** 15-20 Minutes  
**Detailed Guide:** Task-3_Check_IAM_RBAC.md

### What You Will Do

In this task, you will navigate to the Access Control (IAM) section of your Resource Group. You will use the "View my access" feature to see what role your own user account has been assigned. You will then open the Role Assignments tab to see a full list of all identities that have access to your Resource Group, including your user account and your Service Principal. Finally, you will explore the Roles tab to read the definitions of the Owner, Contributor, and Reader roles.

### What You Will See

You will see the Role Assignments list showing at least two entries — one for your user account and one for the Service Principal. Both will be scoped to your Resource Group. The Service Principal entry will show a Type of Service Principal, which visually distinguishes it from human user accounts. In the Roles tab, you will see detailed descriptions of what each built-in role permits and restricts.

### What You Will Learn

- What RBAC (Role-Based Access Control) is and why it exists
- How to check your own access level on an Azure resource
- How to read the Role Assignments list and understand its columns
- The difference between a user identity and a Service Principal identity
- What the Owner, Contributor, and Reader roles permit and restrict
- The principle of Least Privilege — giving only the minimum access required

### Changes Made

None. This task is entirely read-only.

---

## Task 3: Assign a Reader Role and Remove It

**Type:** Hands-On with Full Revert  
**Estimated Time:** 20-25 Minutes  
**Detailed Guide:** Task-4_Assign_Remove_Reader_Role.md

### What You Will Do

In this task, you will perform a hands-on RBAC role assignment. You will manually assign the Reader role to your own user account at the Resource Group scope. After confirming the assignment was successful and observing how the role appears in the Role Assignments list, you will remove the Reader role to restore the access configuration back to its original state.

This task is split into two parts:
- Part A: Assign the Reader role
- Part B: Remove the Reader role (revert)

### What You Will See

After assigning the Reader role, you will see your username appear twice in the Role Assignments list — once with your original role and once with the newly assigned Reader role. This demonstrates that Azure RBAC is additive — you can have multiple roles simultaneously. After removing the Reader role, the list will return to showing your username only once with the original role.

### What You Will Learn

- How to add a role assignment through the Azure Portal
- How the "Add role assignment" wizard works step by step
- That Azure RBAC is additive — multiple roles stack together
- How to remove a specific role assignment without affecting other assignments
- That RBAC changes are immediate — they take effect instantly
- That RBAC changes are fully reversible

### Changes Made

A Reader role assignment is added and then fully removed. The final state of the Role Assignments list is identical to its state before the task began.

---



## Task 4: Locate Your Service Principal Details

**Type:** Read Only  
**Estimated Time:** 15-20 Minutes  
**Detailed Guide:** Task-7_Locate_Service_Principal_Details.md

### What You Will Do

In this task, you will use the Client ID from your Lab Details Page to locate your Service Principal inside Microsoft Entra ID. You will open the App Registration for your Service Principal and verify that the Application (client) ID and Directory (tenant) ID match the values on your Lab Details Page. You will then navigate to the Certificates and Secrets section to observe that the Client Secret value is hidden, and you will understand why Azure never shows secrets after the moment they are created.

### What You Will See

In the App Registrations section under "All applications", you will find your Service Principal using a search with the Client ID. The overview page will show the Application (client) ID and Directory (tenant) ID matching your Lab Details Page exactly. In Certificates and Secrets, you will see the secret entry with its expiry date, but the Value column will display only "Hidden value" — confirming that the secret cannot be retrieved from the portal.

### What You Will Learn

- How to navigate to Microsoft Entra ID in the Azure Portal
- What App Registrations are and how Service Principals are stored there
- How to search for a specific Service Principal using its Client ID
- The anatomy of a Service Principal: Client ID, Client Secret, Tenant ID
- Why Azure hides secret values after creation and the security reasoning behind it
- What a secret expiry date means and the implications of a secret expiring

### Changes Made

None. This task is entirely read-only.

---

## Task 5: Assign RBAC to the Service Principal at Subscription Level 

**Type:** Hands-On with Full Revert  
**Estimated Time:** 25-30 Minutes  
**Detailed Guide:** Task-8_Assign_RBAC_SP_Subscription.md

### What You Will Do

In this task, you will navigate to the Subscriptions page in the Azure Portal and open the Access Control (IAM) section at the Subscription level. You will assign the Contributor role to your pre-provisioned Service Principal at the Subscription scope — a broader level than the Resource Group scope it already has. You will verify the assignment was successful, then navigate to another Resource Group to observe how the Subscription-level role grants inherited access there. 

This task is split into three parts:
- Part A: Note the current Subscription-level role assignments before making changes
- Part B: Assign the Contributor role to the Service Principal at Subscription scope

### What You Will See

Before the task, the Service Principal will not appear in the Subscription-level Role Assignments list — it only has access at the Resource Group level. After assigning the Contributor role, the SP will appear in the Subscription-level list and will also be visible in the Role Assignments of other Resource Groups — showing that Subscription-level roles are inherited by all Resource Groups underneath.

### What You Will Learn

- How to navigate to and work with IAM at the Subscription level
- The difference between Resource Group-level and Subscription-level RBAC scope
- How Subscription-level roles are inherited by all Resource Groups within the Subscription
- Why granting Subscription-level access requires more caution than Resource Group-level access
- How an identity can hold multiple role assignments simultaneously at different scopes
- How removing a Subscription-level role does not affect existing Resource Group-level roles
- The principle of Least Privilege applied at scale — using narrow scopes wherever possible

### Changes Made

The Contributor role is assigned to the Service Principal at the Subscription level and then fully removed. The SP's original Contributor role at the Resource Group level is never touched. After the revert, the SP's permissions are exactly the same as before the task began.

---

## Task 6: Create a Service Connection in Azure DevOps

**Type:** Configuration  
**Estimated Time:** 20-25 Minutes  
**Detailed Guide:** Task-9_Create_Service_Connection.md

### What You Will Do

In this task, you will log in to Azure DevOps for the first time. You will open your pre-created project, navigate to Project Settings, and create a new Service Connection of type Azure Resource Manager using the Service Principal (manual) authentication method. You will enter all four credentials from your Lab Details Page — Subscription ID, Client ID, Client Secret, and Tenant ID. You will then use the built-in Verify function to test the connection and confirm it authenticates successfully before saving.

### What You Will See

After entering all credentials and clicking Verify, you will see a green success indicator confirming that Azure DevOps was able to authenticate to your Azure Subscription using the Service Principal credentials. After saving, the Service Connection will appear in the Service Connections list with a status of Ready. When you open the connection details, you will see all fields except the Client Secret — confirming that Azure DevOps encrypts and stores the secret and never exposes it again after saving.

### What You Will Learn

- How to navigate the Azure DevOps interface and Project Settings
- What a Service Connection is and why it is used in pipelines
- How to create an Azure Resource Manager Service Connection step by step
- What each field in the Service Connection form means and where to get the values
- How the Verify feature tests credentials before saving
- That Azure DevOps encrypts stored secrets — they cannot be retrieved after saving
- How the Service Connection name (azure-sp-connection) is later referenced in pipeline YAML

### Changes Made

A Service Connection named azure-sp-connection is created inside your Azure DevOps project. No Azure resources are created or modified.

---

## Task 7: Run a Pipeline Using the Service Connection

**Type:** Hands-On Pipeline  
**Estimated Time:** 25-30 Minutes  
**Detailed Guide:** Task-10_Run_Pipeline_Service_Connection.md

### What You Will Do

In this task, you will create your first Azure DevOps pipeline. You will initialize your Repos repository, create a YAML file named azure-pipeline.yml, and write a pipeline that uses the AzureCLI@2 task with your Service Connection to authenticate to Azure and run two Azure CLI commands. The first command will display the authenticated account details, and the second will list all resources inside your Resource Group. You will then configure the pipeline, run it manually, and read the live output logs.

### What You Will See

When the pipeline runs, you will watch the logs in real time as the agent is provisioned, authenticates to Azure via your Service Connection, and executes the CLI commands. The output will show a table with your Subscription name and Tenant ID confirming authentication succeeded, followed by a table listing all resources in your Resource Group — including the Linux Virtual Machine — with their names, types, and locations.

### What You Will Learn

- What a YAML pipeline is and how it is structured
- What each section of a pipeline YAML file does (trigger, pool, steps, task)
- How the AzureCLI@2 task works and how it uses the Service Connection for authentication
- What az account show and az resource list commands return
- How to create, commit, and set up a pipeline from an existing YAML file
- How to read and interpret pipeline run logs
- That the Service Connection keeps credentials invisible in both the YAML and the logs

### Changes Made

A YAML file is created in your repository and a pipeline is configured and run. No Azure resources are created, modified, or deleted. The pipeline reads data only.

---

## Task 8: Deploy Azure Resources from ARM Template via Pipeline and Clean Up

**Type:** Hands-On — Full Lifecycle (Create → List → Delete)
**Estimated Time:** 45–55 Minutes
**Detailed Guide:** Task-8_ARM_Template_Pipeline.md

### What You Will Do

In this task, you will build three separate pipelines in Azure DevOps that manage the complete lifecycle of Azure resources using an ARM template stored in a GitHub repository. First, you will write and run a Deploy pipeline that creates a Resource Group, then fetches the ARM template directly from GitHub using its raw URL and deploys a Linux Virtual Machine and a Storage Account. You will then write and run a List pipeline that queries the Resource Group and prints all deployed resources in a table along with a total resource count. Finally, you will write and run a Delete pipeline that records the resources before removal, deletes the entire Resource Group and everything inside it, and confirms the cleanup.

This task is split into five parts:
- Part A: Prepare the ARM template in GitHub and get its raw URL
- Part B: Build and run the Deploy pipeline to create the Resource Group, VM, and Storage Account
- Part C: Build and run the List pipeline to verify all deployed resources
- Part D: Build and run the Delete pipeline to remove all resources
- Part E: Review the complete pipeline run history and confirm all three runs succeeded

### What You Will See

After running the Deploy pipeline, the Azure Portal will show the Resource Group containing a Linux VM and a Storage Account. The pipeline logs will display a resource table with names and types, and confirm the VM is in VM running state and the Storage Account status is available. After running the List pipeline, the logs will display a full breakdown of every resource in the group including networking components, with a total resource count at the bottom. After running the Delete pipeline, the logs will show the pre-deletion resource list, confirm the delete command was sent, and verify the Resource Group no longer exists. Your pipeline Run History in Azure DevOps will show three successful runs — Deploy, List, and Delete.

### What You Will Learn

- How to deploy Azure resources from an ARM template hosted on GitHub using a pipeline
- How to use az deployment group create with --template-uri to reference a remote template without downloading it
- How to pass parameters like VM name and Storage Account name into an ARM deployment from a pipeline script
- How to list and verify Azure resources programmatically using az resource list, az vm list, and az storage account list
- How to safely delete an entire Resource Group and all its resources using az group delete inside a pipeline
- How to use --yes and --no-wait flags in automated delete commands where no human is present to confirm

### Changes Made

A Resource Group, Linux Virtual Machine, Storage Account, and all associated networking resources are created via the Deploy pipeline. The List pipeline makes no changes — it only reads and reports. The Delete pipeline removes the Resource Group and all resources inside it entirely. At the end of this task, the subscription is returned to its original state with no resources remaining. The pipeline YAML file in Repos will reflect the Delete script as its final committed version.

---

## Summary of All Tasks

| Task | What You Practice | Azure Service Used | Makes Changes | Revert Required |
|---|---|---|---|---|
| Task 1 | Portal navigation, Resource Group exploration | Azure Portal, Resource Groups | No | No |
| Task 2 | RBAC role reading, IAM exploration | Access Control (IAM) | No | No |
| Task 3 | Role assignment and removal at RG level | Access Control (IAM), RBAC | Yes | Yes - Remove Reader role from user ||
| Task 4 | Service Principal exploration, Entra ID | Microsoft Entra ID | No | No |
| Task 5 | SP RBAC assignment at Subscription level | Access Control (IAM), Subscriptions, RBAC | Yes | Yes - Remove Reader role from SP |
| Task 6 | Service Connection setup | Azure DevOps Project Settings | Yes (DevOps only) | No |
| Task 7 | YAML pipeline creation and execution | Azure DevOps Repos, Pipelines | Yes (DevOps only) | No |
| Task 8 | Deploy Azure Resources from ARM Template via Pipeline and Clean Up | Azure DevOps Pipelines, Azure CLI, VMs | Yes | Yes - Start VM back to Running |

---

## Before You Begin

Make sure you have the following ready before starting Task 1:

- Your Lab Details Page is open and all values are visible
- You have access to a web browser (Chrome, Edge, or Firefox recommended)
- You have noted down or can refer to the Azure Portal URL, Azure DevOps URL, your username, and your password
- You have read the Lab Overview document to understand the resources and technologies you will be working with

When you are ready, open the Task 1 guide document and begin with Step 1.