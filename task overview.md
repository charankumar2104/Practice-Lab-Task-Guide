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

> [IMAGE PLACEHOLDER - A visual progress bar or numbered step diagram showing all 11 tasks in order from Task 1 to Task 11, with task names labeled under each number]

---

## Task List at a Glance

| Task Number | Task Name | Type | Estimated Time |
|---|---|---|---|
| Task 1 | Explore Your Pre-Provisioned Resource Group | Read Only | 15-20 mins |
| Task 2 | View the Linux VM Details | Read Only | 15-20 mins |
| Task 3 | Check IAM and RBAC on Your Resource Group | Read Only | 15-20 mins |
| Task 4 | Assign a Reader Role and Remove It | Hands-On with Revert | 20-25 mins |
| Task 5 | Export the ARM Template of the Linux VM | Read Only | 15-20 mins |
| Task 6 | Create a New Resource Group and Delete It | Hands-On with Revert | 20-25 mins |
| Task 7 | Locate Your Service Principal Details | Read Only | 15-20 mins |
| Task 8 | Assign RBAC to the Service Principal at Subscription Level and Revert | Hands-On with Revert | 25-30 mins |
| Task 9 | Create a Service Connection in Azure DevOps | Configuration | 20-25 mins |
| Task 10 | Run a Pipeline Using the Service Connection | Hands-On Pipeline | 25-30 mins |
| Task 11 | Start and Stop the VM via Pipeline | Hands-On with Revert | 30-35 mins |

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

> [IMAGE PLACEHOLDER - Screenshot of a Resource Group overview page showing the list of resources inside it and the tags section]

---

## Task 2: View the Linux VM Details

**Type:** Read Only  
**Estimated Time:** 15-20 Minutes  
**Detailed Guide:** Task-2_View_VM_Details.md

### What You Will Do

In this task, you will click on your pre-deployed Linux Virtual Machine inside the Resource Group and explore its properties in detail. You will note down the VM size, operating system, IP addresses, region, and current status. You will then open the JSON View of the VM — which shows the raw ARM representation of the resource — and identify the key sections of an ARM template.

### What You Will See

You will see the full properties of the Virtual Machine including the hardware profile (size), the OS disk details, the network interface it is attached to, and its current power state (Running). The JSON View will show you the exact ARM template structure that was used to deploy this VM, including the resource type, API version, and configuration properties.

### What You Will Learn

- How to read and interpret the overview properties of a Virtual Machine
- What VM size means and how it relates to cost and performance
- What supporting resources a Virtual Machine depends on
- What ARM JSON looks like and what its key sections mean
- The resource provider naming convention used in Azure (e.g., Microsoft.Compute/virtualMachines)

### Changes Made

None. This task is entirely read-only.

---

> [IMAGE PLACEHOLDER - Screenshot of the Virtual Machine overview page showing the key fields: Status, Size, Operating system, and IP addresses all labeled]

---

## Task 3: Check IAM and RBAC on Your Resource Group

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

> [IMAGE PLACEHOLDER - Screenshot of the Access Control (IAM) Role Assignments tab showing entries for both a user account and a service principal with their roles and scope columns visible]

---

## Task 4: Assign a Reader Role and Remove It

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

> [IMAGE PLACEHOLDER - Screenshot showing the Role Assignments list with the participant's username appearing twice - once with the original role and once with Reader - to illustrate the additive nature of RBAC]

---

## Task 5: Export the ARM Template of the Linux VM

**Type:** Read Only  
**Estimated Time:** 15-20 Minutes  
**Detailed Guide:** Task-5_Export_ARM_Template.md

### What You Will Do

In this task, you will use the Export Template feature in the Azure Portal to generate and download the ARM template that represents your pre-deployed Linux Virtual Machine. You will open the downloaded template files and read through the JSON structure, identifying the key sections: parameters, variables, and resources. You will also read the parameters.json file and understand how it works together with the main template file.

### What You Will See

The Export Template page will generate two files: template.json and parameters.json. The template.json file will contain the full ARM definition of all resources in your Resource Group — the Virtual Machine, Network Interface, Virtual Network, Public IP, OS Disk, and Network Security Group. You will be able to see how each resource references other resources through their IDs.

### What You Will Learn

- What an ARM template is and why it is used
- The structure of an ARM template: parameters, variables, resources, and outputs
- How to export an ARM template from an existing resource in the Azure Portal
- How template.json and parameters.json work together
- How Infrastructure as Code (IaC) eliminates the need for manual resource creation
- Why ARM templates enable repeatable, consistent deployments

### Changes Made

None. This task is entirely read-only. Downloading the template does not modify any Azure resource.

---

> [IMAGE PLACEHOLDER - Screenshot of the Export Template page showing the generated template.json content in the editor with the parameters and resources sections visible]

---

## Task 6: Create a New Resource Group and Delete It

**Type:** Hands-On with Full Revert  
**Estimated Time:** 20-25 Minutes  
**Detailed Guide:** Task-6_Create_Delete_Resource_Group.md

### What You Will Do

In this task, you will create a brand new, empty Resource Group inside the Azure Subscription. You will give it a name, assign it to a region, and add tags to it. After confirming it was created successfully and exploring its properties, you will delete the Resource Group to restore the environment to its original state.

This task is split into two parts:
- Part A: Create the new Resource Group
- Part B: Delete the Resource Group (revert)

### What You Will See

After creation, the new Resource Group will appear in the Resource Groups list alongside your original lab Resource Group. It will be completely empty — no resources inside it. The tags you added will be visible on the Tags page. After deletion, the Resource Group will disappear from the list entirely and the environment will look exactly as it did before this task.

### What You Will Learn

- How to create a Resource Group through the Azure Portal
- How to assign a region and add tags during Resource Group creation
- That Resource Groups can be empty and that is a valid state
- What happens when you delete a Resource Group — all contents are deleted with it
- Why verifying an RG is empty before deleting is important in real projects
- How to use the portal's deletion confirmation mechanism

### Changes Made

A new empty Resource Group is created and then fully deleted. The original lab Resource Group is untouched throughout this task.

---

> [IMAGE PLACEHOLDER - Screenshot of the new Resource Group creation form showing the name, region, and tags fields filled in before clicking Create]

---

## Task 7: Locate Your Service Principal Details

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

> [IMAGE PLACEHOLDER - Screenshot of a Service Principal App Registration overview page in Microsoft Entra ID with Application (client) ID and Directory (tenant) ID fields highlighted]

---

## Task 8: Assign RBAC to the Service Principal at Subscription Level and Revert

**Type:** Hands-On with Full Revert  
**Estimated Time:** 25-30 Minutes  
**Detailed Guide:** Task-8_Assign_RBAC_SP_Subscription.md

### What You Will Do

In this task, you will navigate to the Subscriptions page in the Azure Portal and open the Access Control (IAM) section at the Subscription level. You will assign the Reader role to your pre-provisioned Service Principal at the Subscription scope — a broader level than the Resource Group scope it already has. You will verify the assignment was successful, then navigate to another Resource Group to observe how the Subscription-level role grants inherited access there. Finally, you will remove the Subscription-level Reader role to restore the SP back to only its original Resource Group-level Contributor role.

This task is split into three parts:
- Part A: Note the current Subscription-level role assignments before making changes
- Part B: Assign the Reader role to the Service Principal at Subscription scope
- Part C: Remove the Subscription-level Reader role (revert to original state)

### What You Will See

Before the task, the Service Principal will not appear in the Subscription-level Role Assignments list — it only has access at the Resource Group level. After assigning the Reader role, the SP will appear in the Subscription-level list and will also be visible in the Role Assignments of other Resource Groups — showing that Subscription-level roles are inherited by all Resource Groups underneath. After removal, the SP will disappear from the Subscription-level list and from other Resource Groups, while its original Contributor role on your own Resource Group remains completely unchanged.

### What You Will Learn

- How to navigate to and work with IAM at the Subscription level
- The difference between Resource Group-level and Subscription-level RBAC scope
- How Subscription-level roles are inherited by all Resource Groups within the Subscription
- Why granting Subscription-level access requires more caution than Resource Group-level access
- How an identity can hold multiple role assignments simultaneously at different scopes
- How removing a Subscription-level role does not affect existing Resource Group-level roles
- The principle of Least Privilege applied at scale — using narrow scopes wherever possible

### Changes Made

The Reader role is assigned to the Service Principal at the Subscription level and then fully removed. The SP's original Contributor role at the Resource Group level is never touched. After the revert, the SP's permissions are exactly the same as before the task began.

---

> [IMAGE PLACEHOLDER - Screenshot of the Subscription-level Access Control (IAM) Role Assignments tab showing the Service Principal listed with Reader role and Scope = This resource (Subscription), then a second screenshot showing the SP removed from the list after the revert]

---

## Task 9: Create a Service Connection in Azure DevOps

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

> [IMAGE PLACEHOLDER - Screenshot of the Service Connection form filled in with the four credential fields (Subscription ID, Client ID, Tenant ID visible - Client Secret field blurred) and the Verify button highlighted]

---

## Task 10: Run a Pipeline Using the Service Connection

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

> [IMAGE PLACEHOLDER - Screenshot of the pipeline run logs showing the successful output of az account show and az resource list with the VM visible in the resource list output]

---

## Task 11: Start and Stop the VM via Pipeline

**Type:** Hands-On with Full Revert  
**Estimated Time:** 30-35 Minutes  
**Detailed Guide:** Task-11_Start_Stop_VM_Pipeline.md

### What You Will Do

In this task, you will modify your existing pipeline twice. First, you will replace the pipeline content with a script that stops your Linux Virtual Machine using the az vm stop command. You will run this pipeline, watch the logs, and verify in the Azure Portal that the VM changes to a Stopped (deallocated) state. You will then update the pipeline again with a script that starts the VM using the az vm start command, run it, and confirm in the Azure Portal that the VM returns to Running state.

This task is split into three parts:
- Part A: Verify the VM is in Running state before you begin
- Part B: Modify and run the pipeline to stop the VM
- Part C: Modify and run the pipeline to start the VM and restore it to Running state

### What You Will See

After running the Stop pipeline, the Azure Portal will show the VM status as Stopped (deallocated). The pipeline logs will show the output VM deallocated confirming the operation completed. After running the Start pipeline, the Azure Portal will show the VM status return to Running. The pipeline logs will show VM running confirming the restore is complete. Your pipeline Run History in Azure DevOps will show two successful runs — one for Stop and one for Start.

### What You Will Learn

- The difference between VM power states: Running, Stopped, and Deallocated
- Why Deallocated stops compute billing while Stopped (OS level) does not
- How to use az vm stop and az vm start commands inside a pipeline
- How real teams use scheduled pipelines to shut down non-production VMs to save costs
- How to read pipeline run history and compare multiple runs
- How to verify infrastructure state changes in the Azure Portal after a pipeline run

### Changes Made

The VM is stopped via pipeline and then started via pipeline. At the end of this task, the VM is fully restored to Running state — identical to its state before the task began. The pipeline YAML file in Repos will reflect the Start script as its final committed version.

---

> [IMAGE PLACEHOLDER - Screenshot split or side by side showing: left side - Azure Portal VM status showing "Stopped (deallocated)" after the Stop pipeline, and right side - Azure Portal VM status showing "Running" after the Start pipeline]

---

## Summary of All Tasks

| Task | What You Practice | Azure Service Used | Makes Changes | Revert Required |
|---|---|---|---|---|
| Task 1 | Portal navigation, Resource Group exploration | Azure Portal, Resource Groups | No | No |
| Task 2 | VM properties, ARM JSON view | Azure Virtual Machines | No | No |
| Task 3 | RBAC role reading, IAM exploration | Access Control (IAM) | No | No |
| Task 4 | Role assignment and removal at RG level | Access Control (IAM), RBAC | Yes | Yes - Remove Reader role from user |
| Task 5 | ARM template export and reading | Azure Virtual Machines, ARM | No | No |
| Task 6 | Resource Group creation and deletion | Resource Groups | Yes | Yes - Delete the new RG |
| Task 7 | Service Principal exploration, Entra ID | Microsoft Entra ID | No | No |
| Task 8 | SP RBAC assignment at Subscription level | Access Control (IAM), Subscriptions, RBAC | Yes | Yes - Remove Reader role from SP |
| Task 9 | Service Connection setup | Azure DevOps Project Settings | Yes (DevOps only) | No |
| Task 10 | YAML pipeline creation and execution | Azure DevOps Repos, Pipelines | Yes (DevOps only) | No |
| Task 11 | VM start/stop via pipeline automation | Azure DevOps Pipelines, Azure CLI, VMs | Yes | Yes - Start VM back to Running |

---

## Before You Begin

Make sure you have the following ready before starting Task 1:

- Your Lab Details Page is open and all values are visible
- You have access to a web browser (Chrome, Edge, or Firefox recommended)
- You have noted down or can refer to the Azure Portal URL, Azure DevOps URL, your username, and your password
- You have read the Lab Overview document to understand the resources and technologies you will be working with

When you are ready, open the Task 1 guide document and begin with Step 1.