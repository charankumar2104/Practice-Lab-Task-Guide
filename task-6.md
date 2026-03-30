# Task 6: Run a Pipeline Using the Service Connection

**Difficulty:** Intermediate  
**Estimated Time:** 25-30 Minutes  
**Type:** Hands-On Configuration (Creates a pipeline and runs it — does not modify any Azure resources)

---

## Objective

In this task, you will create a YAML pipeline inside Azure DevOps, connect it to your Azure subscription using the Service Connection you created in Task 8, and run it. The pipeline will authenticate to Azure using your Service Principal and list the resources inside your Resource Group. This is a read-only pipeline — it will not create, modify, or delete anything in Azure.

---

## Pre-Requisites

Before starting this task, make sure you have completed:
- Task 4: Locate Your Service Principal Details
- Task 5: Create a Service Connection in Azure DevOps

Have the following ready from your Lab Environment Page:

| Item | Where to Find It |
|---|---|
| Azure DevOps Organization URL | Lab Environment Page |
| Azure DevOps Username | Lab Environment Page |
| Azure DevOps Password | Lab Environment Page |
| Your Resource Group Name | Lab Environment Page (e.g., rg-participant-john) |
| Service Connection Name | azure-sp-connection (created in Task 8) |

---

## Quick Background: What Is a YAML Pipeline?

A pipeline in Azure DevOps is a set of automated instructions that run in sequence. In modern Azure DevOps, pipelines are written in a format called **YAML** (Yet Another Markup Language) — a simple text-based format that uses indentation to define structure.

When you run a pipeline:

1. Azure DevOps reads your YAML file from the repository
2. It provisions a temporary virtual machine called an **agent** to run your steps
3. The agent executes each step in order
4. Logs are captured and shown to you in real time
5. The agent is discarded after the pipeline finishes

The pipeline in this task will use the **AzureCLI** task — a built-in Azure DevOps task that authenticates to Azure using your Service Connection and then runs Azure CLI commands on your behalf.

---

## Step-by-Step Instructions

---

### Step 1: Log In to Azure DevOps

1. Open your web browser
2. In the address bar, type the Azure DevOps Organization URL from your Lab Details Page
   - Example: `https://dev.azure.com/contoso-lab`
3. Press Enter
4. Enter your Azure DevOps Username and click Next
5. Enter your Azure DevOps Password and click Sign in
6. If prompted with "Stay signed in?", click No

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure DevOps sign-in page with the username and password fields visible]

---

### Step 2: Open Your Project

1. After signing in, you will land on the Azure DevOps organization home page
2. You will see your project listed (e.g., contoso-lab-john)
3. Click on your project name to open it

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure DevOps organization home page with the participant's project name visible and highlighted]

---

### Step 3: Navigate to Repos

1. On the left sidebar of your project, look for the navigation menu
2. Click on **Repos**
3. The Repos section will open — this is where your code and pipeline files are stored

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure DevOps project left sidebar with "Repos" highlighted and clicked]

---

### Step 4: Initialize the Repository

1. If the repository is empty, you will see a page that says "Initialize your repository" or "This repository is empty"
2. Scroll down and find the **Initialize** button
3. Make sure the option to add a README is checked
4. Click the **Initialize** button

> Note: If the repository already has files in it, skip this step and proceed directly to Step 5.

---

> [IMAGE PLACEHOLDER - Screenshot of the empty repository page showing the Initialize button and the option to add a README file]

---

### Step 5: Confirm the Repository Is Initialized

1. After initializing, you will see the repository with at least one file — usually a README.md
2. The file tree will be visible on the left
3. You are now ready to create a new pipeline file

---

> [IMAGE PLACEHOLDER - Screenshot of the initialized repository showing the README.md file in the file listing]

---

### Step 6: Create a New File in the Repository

1. On the Repos page, look at the top-right area near the file listing
2. Click on the three dots (...) or the **+ Add** button/dropdown near the top of the file listing area
3. From the dropdown, select **New file**

---

> [IMAGE PLACEHOLDER - Screenshot showing the "+ Add" button or three dots menu in the Repos section with "New file" highlighted in the dropdown]

---

### Step 7: Name the New File

1. A dialog box or an inline text field will appear asking for the file name
2. Type the following name exactly:

```
azure-pipeline.yml
```

3. Make sure the name ends with `.yml` — this is important for Azure DevOps to recognize it as a pipeline file
4. Click **Create** or press Enter to confirm

---

> [IMAGE PLACEHOLDER - Screenshot of the new file dialog with "azure-pipeline.yml" typed in the filename field and the Create button visible]

---

### Step 8: Enter the Pipeline Code

1. The file editor will open with a blank canvas
2. Click inside the editor area
3. First, clear any default content if any is present
4. Type or paste the following YAML content exactly as shown below

> Important: YAML is sensitive to indentation. Every space matters. Do not use tabs — use spaces only. Copy and paste is recommended to avoid errors.

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:
- task: AzureCLI@2
  displayName: 'Authenticate and List Resources'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Authenticated Account Details"
      echo "============================================"
      az account show --output table
      echo ""
      echo "============================================"
      echo "Resources Inside My Resource Group"
      echo "============================================"
      az resource list --resource-group YOUR-RESOURCE-GROUP-NAME --output table
```

5. In the last line, replace `YOUR-RESOURCE-GROUP-NAME` with your actual Resource Group name from the Lab Details Page
   - Example: if your RG is `rg-participant-john`, the line should read:
   - `az resource list --resource-group rg-participant-john --output table`

---

> [IMAGE PLACEHOLDER - Screenshot of the file editor in Azure Repos showing the YAML content pasted in, with the resource group name replaced correctly]

---

### Step 9: Understand What Each Line of the Pipeline Does

Before saving, take a moment to understand what you have written:

| Line / Section | What It Does |
|---|---|
| `trigger: none` | This pipeline will NOT run automatically. It only runs when you manually trigger it |
| `pool: vmImage: ubuntu-latest` | Azure DevOps will create a temporary Ubuntu Linux virtual machine to run this pipeline |
| `task: AzureCLI@2` | This is a built-in task that authenticates to Azure and then runs Azure CLI commands |
| `azureSubscription: 'azure-sp-connection'` | This tells the task to use the Service Connection you created in Task 8 to authenticate |
| `scriptType: bash` | The script will be written in Bash (Linux shell language) |
| `az account show` | This Azure CLI command prints the details of the authenticated account (SP identity) |
| `az resource list` | This Azure CLI command lists all resources inside your specified Resource Group |

---

> [IMAGE PLACEHOLDER - Screenshot of the YAML editor with annotations or callouts pointing to the key sections: trigger, pool, azureSubscription, and the az commands]

---

### Step 10: Commit the File to the Repository

1. After entering the YAML content, look for the **Commit** button at the top right of the editor
2. Click **Commit**
3. A dialog box will appear asking for a commit message
4. In the commit message field, type:

```
Add initial pipeline file for resource listing
```

5. Leave the branch as **main** (or **master** — whichever is shown)
6. Make sure "Commit directly to the main branch" is selected
7. Click the **Commit** button inside the dialog to save

---

> [IMAGE PLACEHOLDER - Screenshot of the Commit dialog showing the commit message typed in, branch set to main, and the Commit button highlighted]

---

### Step 11: Confirm the File Was Committed

1. After committing, you will be taken back to the Repos file listing
2. You should now see two files in your repository:
   - README.md
   - azure-pipeline.yml
3. Click on **azure-pipeline.yml** to confirm its contents look correct

---

> [IMAGE PLACEHOLDER - Screenshot of the repository file listing showing both README.md and azure-pipeline.yml files, with azure-pipeline.yml highlighted]

---

### Step 12: Navigate to Pipelines

1. In the left sidebar, click on **Pipelines**
2. The Pipelines section will open
3. If no pipelines exist yet, you will see a "Create Pipeline" prompt or an empty list
4. Click **New pipeline** or **Create Pipeline**

---

> [IMAGE PLACEHOLDER - Screenshot of the Pipelines section in the left sidebar selected and the empty pipelines page with "New pipeline" button visible]

---

### Step 13: Select the Code Source

1. A page titled "Where is your code?" will appear
2. You will see several source options:
   - Azure Repos Git
   - GitHub
   - Bitbucket Cloud
   - Other Git
3. Click on **Azure Repos Git** since your YAML file is stored in the Azure Repos repository you just edited

---

> [IMAGE PLACEHOLDER - Screenshot of the "Where is your code?" page with "Azure Repos Git" option highlighted and selected]

---

### Step 14: Select Your Repository

1. A list of repositories in your project will appear
2. Click on your repository name (it will match your project name, e.g., contoso-lab-john)

---

> [IMAGE PLACEHOLDER - Screenshot of the repository selection page showing the participant's repository name listed and highlighted]

---

### Step 15: Select the Existing YAML File

1. The next page will ask "Configure your pipeline"
2. You will see several options:
   - Starter pipeline
   - Existing Azure Pipelines YAML file
   - Other options
3. Click on **Existing Azure Pipelines YAML file**

---

> [IMAGE PLACEHOLDER - Screenshot of the Configure pipeline page with "Existing Azure Pipelines YAML file" option highlighted]

---

### Step 16: Select the azure-pipeline.yml File

1. A panel will slide open on the right
2. In the **Branch** dropdown, make sure **main** (or master) is selected
3. In the **Path** dropdown, select **/azure-pipeline.yml**
4. Click the **Continue** button

---

> [IMAGE PLACEHOLDER - Screenshot of the file selector panel with branch = main and Path = /azure-pipeline.yml selected, with the Continue button highlighted]

---

### Step 17: Review the Pipeline YAML Before Running

1. Azure DevOps will show you a preview of your YAML file
2. Read through it and confirm:
   - `azureSubscription` says `azure-sp-connection`
   - The resource group name in the last line matches your actual RG name
3. If everything looks correct, click the **Run** button at the top right

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline YAML review page showing the full YAML content and the Run button highlighted at the top right]

---

### Step 18: Watch the Pipeline Run in Real Time

1. After clicking Run, you will be taken to the **Pipeline Run** page
2. You will see a summary showing:
   - The job status (Queued, Running, Succeeded, Failed)
   - A job named "Job" under the run
3. Click on the **Job** link to see the detailed logs
4. Watch as each step executes in real time:
   - First, the agent is provisioned (this takes about 30-60 seconds)
   - Then "Authenticate and List Resources" step runs
   - Output will appear in the log panel

---

> [IMAGE PLACEHOLDER - Screenshot of the Pipeline Run page showing the job status as "Running" with the Job link highlighted]

---

### Step 19: Read the Pipeline Output Logs

1. After the job completes (status changes to "Succeeded" with a green checkmark), click on the step named **"Authenticate and List Resources"**
2. The log output will expand and you should see something like this:

```
============================================
Authenticated Account Details
============================================
EnvironmentName    HomeTenantId    IsDefault    Name                              State    TenantId
-----------------  --------------  -----------  --------------------------------  -------  --------
Azure              xxxxxxxx-xxxx   True         Contoso-Training-Subscription     Enabled  xxxxxxxx

============================================
Resources Inside My Resource Group
============================================
Name              ResourceGroup         Location    Type
----------------  --------------------  ----------  ----------------------------------
your-vm-name      rg-participant-john   eastus      Microsoft.Compute/virtualMachines
```

3. Confirm the following in the output:
   - The subscription name matches what is on your Lab Details Page
   - Your Resource Group name appears in the resources list
   - Your Linux VM is listed as a resource of type `Microsoft.Compute/virtualMachines`

---

> [IMAGE PLACEHOLDER - Screenshot of the expanded pipeline log output showing the az account show table and az resource list table with the VM listed]

---

### Step 20: Confirm the Pipeline Run Status Is "Succeeded"

1. Click the back arrow or the pipeline name breadcrumb to go back to the run summary page
2. The overall status should show **Succeeded** with a green checkmark
3. Note the run duration shown — this tells you how long the pipeline took from start to finish

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline run summary page showing "Succeeded" status with a green indicator and the run duration visible]

---

### Step 21: Verify No Azure Resources Were Changed

1. Open a new browser tab
2. Go to `https://portal.azure.com`
3. Navigate to your Resource Group
4. Confirm the resources inside are exactly the same as before you ran the pipeline
5. No new resources have been created — the pipeline only read data, it did not create anything

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure Portal Resource Group overview confirming the same resources exist as before the pipeline was run]

---

## Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
[ ] Logged in to Azure DevOps successfully
[ ] Opened your project and navigated to Repos
[ ] Initialized the repository if it was empty
[ ] Created a new file named azure-pipeline.yml
[ ] Entered the YAML content correctly with the right resource group name
[ ] Committed the file to the main branch
[ ] Navigated to Pipelines and created a new pipeline
[ ] Selected Azure Repos Git as the code source
[ ] Selected the existing azure-pipeline.yml file
[ ] Reviewed the YAML and clicked Run
[ ] Watched the pipeline run in real time
[ ] Read the logs and confirmed the subscription name and VM appeared
[ ] Confirmed the pipeline status shows Succeeded
[ ] Verified no Azure resources were created or changed
```
---
