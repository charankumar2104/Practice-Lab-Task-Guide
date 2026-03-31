# Lab Overview: Azure DevOps with Azure Infrastructure

**Lab Title:** Azure DevOps with Azure Infrastructure - End-to-End Hands-On Lab  
**Client:** Contoso Innovations Inc.  
**Difficulty:** Beginner to Intermediate  
**Total Tasks:** 8  
**Total Estimated Duration:** 8 to 10 Hours  
**Lab Type:** Guided Hands-On

---

## About This Lab

This lab is designed to give you practical, real-world experience working with Microsoft Azure and Azure DevOps. You will not just read about these technologies — you will use them directly through a browser-based environment that has been pre-configured specifically for you.

Each participant in this lab receives their own isolated environment. This means everything you do stays within your own assigned space and does not affect any other participant. You are free to explore, make changes, and learn without the risk of breaking anything that belongs to someone else.

By the end of this lab, you will have worked hands-on with core Azure services, understood how access and identity management works at both the Resource Group and Subscription level, assigned and managed RBAC roles for a Service Principal, and built and run automated pipelines that interact with real Azure infrastructure.

---

## How the Lab Environment Works

When you receive your Lab Details Page, your environment has already been provisioned and is ready for you. The following things were automatically set up before you arrived:

- A dedicated Azure Resource Group scoped to your name
- A Linux Virtual Machine deployed inside that Resource Group using an ARM template
- A Service Principal created in Microsoft Entra ID with Contributor access to your Resource Group
- All Service Principal credentials captured and displayed on your Lab Environments Page
- An Azure DevOps Organization and a project pre-created for you

You do not need to set up any of the above. Your job is to explore, understand, use, and build on top of what has been provisioned.

---

## Lab Environment Details

At the start of the lab, your Lab Details Page will contain the following information. Keep this page open throughout the entire lab as you will refer to it many times.

| Item | Description |
|---|---|
| Azure Portal URL | The web address to access the Azure Portal |
| Azure Username | Your personal login for the Azure Portal |
| Azure Password | Your password for the Azure Portal |
| Resource Group Name | The name of your pre-created Resource Group |
| Azure DevOps Organization URL | The web address for your Azure DevOps environment |
| Azure DevOps Username | Your login for Azure DevOps |
| Azure DevOps Password | Your password for Azure DevOps |
| Tenant ID | The unique identifier of the Azure Active Directory tenant |
| Subscription ID | The unique identifier of the Azure Subscription |
| Subscription Name | The display name of the Azure Subscription |
| Client ID | The Application ID of your pre-created Service Principal |
| Client Secret | The secret/password of your pre-created Service Principal |
| VM Name | The name of your pre-deployed Linux Virtual Machine |

---

## Key Technologies and Resources You Will Work With

Below is a plain-language definition of every technology and resource you will encounter during this lab. Read through these definitions before starting the tasks so that you have a baseline understanding of what each thing is.

---

### Microsoft Azure

Microsoft Azure is a cloud computing platform provided by Microsoft. It allows individuals and organizations to rent computing resources — such as virtual machines, storage, databases, and networking — over the internet rather than owning and managing physical hardware themselves.

In this lab, Azure is the platform where your infrastructure lives. You will interact with it through the Azure Portal, which is a web-based graphical interface.

---

### Azure Portal

The Azure Portal is the web-based interface you use to manage all of your Azure resources. You access it by going to https://portal.azure.com in a browser. From the portal, you can create, view, modify, and delete Azure resources using a graphical point-and-click interface without needing to write any code.

In this lab, you will use the Azure Portal to explore your Resource Group, check access controls, view your Virtual Machine, and verify changes made by your pipelines.

---

### Azure Subscription

An Azure Subscription is the billing and access boundary in Azure. Everything you create in Azure — virtual machines, storage accounts, databases — must belong to a Subscription. The Subscription is how Microsoft tracks usage and sends invoices.

In this lab, your Resource Group and all resources inside it belong to a pre-existing Subscription that has been provided to you. You will see the Subscription name and Subscription ID on your Lab Details Page.

---

### Azure Resource Group

A Resource Group is a logical container inside an Azure Subscription that holds related Azure resources together. Think of it like a folder on your computer — the folder itself does not do anything, but it organizes the files (resources) inside it.

Key characteristics of a Resource Group:
- Every Azure resource must belong to exactly one Resource Group
- Deleting a Resource Group deletes everything inside it
- You can assign access permissions (RBAC) at the Resource Group level
- Resource Groups are tied to a region (geographic location)
- Tags can be applied to a Resource Group for organization and billing tracking

In this lab, your Resource Group has been pre-created with a name like rg-participant-yourname. All your lab resources — including the Linux VM — live inside it.

---

### Azure Virtual Machine (VM)

An Azure Virtual Machine is a software-based computer running inside Microsoft's data centers. It behaves exactly like a physical computer — it has an operating system, memory, CPU, storage, and a network connection — but it exists entirely in software and runs in the cloud.

Virtual Machines are one of the most fundamental services in Azure. They are used to run applications, host websites, process data, and much more.

In this lab, a Linux Virtual Machine has been pre-deployed inside your Resource Group using an ARM template. You will explore its properties, check its status, and control its power state (start and stop) using automated pipelines.

Key VM terms you will encounter:
- **VM Size:** Defines how much CPU and memory the VM has (e.g., Standard_B1s is a basic size suitable for lab use)
- **OS Disk:** The storage disk attached to the VM that holds the operating system
- **Network Interface (NIC):** The virtual network card that connects the VM to a network
- **Public IP Address:** The IP address used to reach the VM from the internet
- **Power State:** Whether the VM is Running, Stopped, or Deallocated

---

### ARM Template (Azure Resource Manager Template)

An ARM Template is a JSON (JavaScript Object Notation) file that describes the infrastructure you want to deploy in Azure. Instead of manually clicking through the Azure Portal to create resources one by one, you write a template that defines all the resources, and Azure reads that template and creates everything automatically.

ARM Templates are the foundation of Infrastructure as Code (IaC) in Azure — the practice of managing cloud infrastructure through code files rather than manual actions.

In this lab, the Linux Virtual Machine in your Resource Group was deployed using an ARM template. You will explore the template structure in the Azure Portal and learn how to read its key sections: parameters, variables, and resources.

---

### Azure Role-Based Access Control (RBAC)

RBAC (Role-Based Access Control) is the system Azure uses to control who is allowed to do what on which resources. Every action in Azure — reading a resource, creating a VM, assigning a role — requires the user or application performing it to have a role that permits that action.

RBAC in Azure works by assigning a Role to an Identity at a Scope:

- **Role:** What actions are permitted (e.g., Reader, Contributor, Owner)
- **Identity:** Who is being given the permission (a user account, a group, or a Service Principal)
- **Scope:** Where the permission applies (Tenant, Subscription, Resource Group, or individual resource)

The three most commonly used built-in roles are:

| Role | Permissions |
|---|---|
| Owner | Full control over resources and can assign roles to other identities |
| Contributor | Can create and manage resources but cannot assign roles to others |
| Reader | Can only view resources — cannot create, modify, or delete anything |

Understanding RBAC scope is especially important in this lab. The same role can be assigned at different levels, and the level at which it is assigned determines how wide the access is:

| Scope | What Access Is Granted |
|---|---|
| Tenant | Across all Subscriptions and everything inside them |
| Subscription | Across all Resource Groups and resources within that Subscription |
| Resource Group | Only within that specific Resource Group and its contents |
| Individual Resource | Only on that single resource — nothing else |

Roles assigned at a higher scope are automatically inherited by everything underneath. For example, a Reader role assigned at the Subscription level means the identity can read resources in every Resource Group in that Subscription — even ones they were never explicitly added to.

In this lab, you will explore the RBAC assignments on your Resource Group, practice assigning and removing a user-level role, and then perform a more advanced task of assigning and removing a role on your Service Principal at the Subscription level — observing how Subscription-scope access differs from Resource Group-scope access.

---

### RBAC Scope Inheritance

When a role is assigned at the Subscription level, that role is inherited by all Resource Groups and all individual resources inside the Subscription. This is called scope inheritance. It means you do not need to go into each Resource Group and manually add the identity — assigning it once at the Subscription level covers everything underneath automatically.

This is a powerful but risky feature. Granting a high-privilege role like Owner or Contributor at the Subscription level gives extremely broad access. In real organizations, Subscription-level role assignments are reserved for platform administrators, and individual teams are only given access at their own Resource Group level. Following the Least Privilege principle — granting the minimum access required and at the narrowest scope needed — is a core security best practice in Azure.

---

### Microsoft Entra ID (Azure Active Directory)

Microsoft Entra ID (formerly known as Azure Active Directory or Azure AD) is Microsoft's cloud-based identity and access management service. It is the system that manages all identities in Azure — including user accounts, groups, and Service Principals.

When you log in to the Azure Portal with your username and password, Entra ID is what verifies your identity. When a pipeline authenticates to Azure using a Client ID and Client Secret, Entra ID is what validates those credentials.

In this lab, you will navigate to Microsoft Entra ID to find your Service Principal in the App Registrations section.

---

### Service Principal

A Service Principal is a type of identity in Microsoft Entra ID that is intended for use by applications, automation scripts, and pipelines — not by human users. It is sometimes described as a service account for non-human workloads.

A Service Principal authenticates using two pieces of information:
- **Client ID (Application ID):** This is the username — a unique identifier in GUID format
- **Client Secret:** This is the password — a string value that proves the identity

Service Principals are used because it is bad practice to use a real person's credentials in automated pipelines. If a person leaves the organization, their account gets disabled and every pipeline using their credentials breaks. A Service Principal exists independently of any person and can be managed, rotated, and audited separately.

In this lab, a Service Principal has been pre-created for you. Its credentials are visible on your Lab Details Page. You will locate it in Entra ID, explore its properties, assign it a role at the Subscription level, remove that role to restore the original state, and then use its credentials to create a Service Connection in Azure DevOps.

---

### Client Secret

A Client Secret is the password of a Service Principal. It is a long, randomly generated string that is paired with the Client ID to authenticate the Service Principal to Microsoft Entra ID.

An important security behavior of Client Secrets: Azure only shows the secret value once — at the moment it is created. After that, it is stored encrypted and the value can never be retrieved again, even by administrators. This is why the secret is shown on your Lab Details Page — it was captured by the automation script at the exact moment the secret was generated.

Client Secrets have an expiry date. In production environments, secrets must be rotated before they expire, or any pipeline using them will fail to authenticate.

---

### Tenant ID

The Tenant ID is the unique identifier of your Microsoft Entra ID directory (also called a tenant). Every organization that uses Azure or Microsoft 365 has a unique Tenant ID. It is a GUID (Globally Unique Identifier) in the format xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.

The Tenant ID is required when creating a Service Connection in Azure DevOps because it tells Azure DevOps which Entra ID directory to authenticate against.

---

### Azure DevOps

Azure DevOps is a set of developer tools provided by Microsoft that supports the full software development and deployment lifecycle. It includes tools for managing code, planning work, running automated tests, and deploying infrastructure and applications.

In this lab, you will use two specific parts of Azure DevOps:
- **Repos:** Where your pipeline code files are stored
- **Pipelines:** Where you create, configure, and run automated pipelines

---

### Azure DevOps Repository (Repos)

A Repository in Azure DevOps Repos is a version-controlled storage space for your code files. When you create or edit a file in Repos and commit it, Azure DevOps records that change in the history. You can always go back and see what changed, when, and why.

In this lab, you will create a YAML file in your Repos repository. This file will serve as your pipeline definition — the set of instructions Azure DevOps will follow when you run the pipeline.

---

### Azure DevOps Pipeline

An Azure DevOps Pipeline is an automated workflow that runs a set of defined steps in sequence. Modern Azure DevOps pipelines are written in YAML format and stored in your repository alongside your code. This approach is called Pipeline as Code — because the pipeline definition itself is a code file that can be version-controlled and reviewed like any other file.

When a pipeline runs, Azure DevOps provisions a temporary agent (a virtual machine) that executes each step defined in the YAML file and then discards the agent after the run.

In this lab, you will create a YAML pipeline that uses Azure CLI commands to interact with your Azure subscription — listing resources, stopping a VM, and starting it again.

---

### YAML

YAML stands for "Yet Another Markup Language." It is a simple, human-readable text format used to write configuration files. In Azure DevOps, pipeline definitions are written in YAML.

YAML uses indentation (spaces) to define structure. This means that the number of spaces at the beginning of each line is significant — misaligned lines will cause errors.

Example of what YAML looks like:

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:
- task: AzureCLI@2
  inputs:
    azureSubscription: 'azure-sp-connection'
```

In this lab, you will write and modify YAML pipeline files. Pre-written templates are provided for you so you do not need to write YAML from scratch.

---

### Service Connection

A Service Connection is a saved, named, encrypted credential configuration stored inside an Azure DevOps project. It allows Azure DevOps pipelines to authenticate and communicate with external services — such as Azure, GitHub, Docker Hub, or others — without having raw credentials hardcoded in the pipeline code.

When a pipeline references a Service Connection by name (for example, azureSubscription: 'azure-sp-connection'), Azure DevOps retrieves the stored credentials, authenticates to the external service on behalf of the pipeline, and executes the task. The credentials themselves are never visible in the pipeline logs or code.

In this lab, you will create a Service Connection that uses your pre-provisioned Service Principal credentials to link Azure DevOps to your Azure Subscription.

---

### Azure CLI (Command Line Interface)

The Azure CLI is a command-line tool provided by Microsoft that allows you to manage Azure resources by typing commands instead of clicking through the Azure Portal. Azure CLI commands begin with az and follow a consistent structure.

Examples:
- `az resource list` — lists resources in a Resource Group
- `az vm stop` — stops a Virtual Machine
- `az vm start` — starts a Virtual Machine
- `az account show` — displays details of the currently authenticated account

In this lab, you will use Azure CLI commands inside your pipeline YAML files. The Azure DevOps AzureCLI@2 task handles authentication automatically using the Service Connection and then runs your specified CLI commands.

---

### AzureCLI@2 Task

AzureCLI@2 is a built-in Azure DevOps pipeline task that combines two actions into one:
1. It authenticates to Azure using the specified Service Connection
2. It runs your specified Azure CLI (bash or PowerShell) commands after authentication

By using this task, you do not need to manually log in to Azure inside your pipeline. The authentication is handled transparently through the Service Connection.

In this lab, all pipeline scripts use the AzureCLI@2 task with bash scripts.

---

## Lab Architecture Summary

The diagram below shows how all the components in this lab connect to each other:

```
Microsoft Entra ID (Azure AD)
        |
        |-- Service Principal (Client ID + Client Secret)
        |         |
        |         |-- Pre-assigned: Contributor Role on Resource Group (rg-participant-yourname)
        |         |-- Task 8: Reader Role assigned at Subscription level (then removed)
        |
Azure Subscription
        |
        |-- Resource Group (rg-participant-yourname)
        |         |
        |         |-- Linux Virtual Machine
        |         |-- Network Interface
        |         |-- Virtual Network
        |         |-- Public IP Address
        |         |-- OS Disk
        |         |-- Network Security Group
        |
        |-- Other Resource Groups (visible to SP during Task 8 via Subscription-level Reader role)

Azure DevOps Organization
        |
        |-- Project (contoso-lab-yourname)
                  |
                  |-- Repos (azure-pipeline.yml)
                  |
                  |-- Pipelines
                  |         |
                  |         |-- Uses Service Connection
                  |
                  |-- Service Connection (azure-sp-connection)
                            |
                            |-- References the Service Principal credentials
                            |-- Connects to the Azure Subscription
```

---

> [IMAGE PLACEHOLDER - Visual architecture diagram showing all the above components as labeled boxes with arrows showing the relationships and connections between them]

---

## Important Rules for This Lab

- Do not modify the permissions of your lab user account beyond what is instructed in the tasks. Every task that asks you to make a change also asks you to revert it.
- Do not attempt to access other participants' Resource Groups or DevOps projects.
- Do not delete the pre-deployed Linux Virtual Machine or the original Resource Group.
- Do not create a new Client Secret for the Service Principal — the one provided on the Lab Details Page is what you need.
- If you encounter an access denied error that is not expected as part of a task, notify your instructor.

---

## What Happens to the Environment After the Lab

After the lab session ends, the entire environment — the Resource Group, the Virtual Machine, the Service Principal, and the Azure DevOps project — will be automatically cleaned up by the lab provisioning system. You do not need to manually delete anything at the end of the lab unless explicitly instructed to do so during a specific task.

---

## Proceed to the Task Overview

Once you have read and understood this overview, proceed to the **Task Overview** document which lists all the tasks you will perform in this lab, in order.