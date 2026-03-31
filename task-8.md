# Task 8: Deploy Azure Resources from ARM Template via Pipeline and Clean Up

**Difficulty:** Intermediate  
**Estimated Time:** 45–55 Minutes  
**Type:** Hands-On — Full Lifecycle (Create → List → Delete)

---

## Objective

In this task, you will build **three pipelines** in Azure DevOps that manage the full lifecycle of Azure resources using an ARM template hosted on GitHub:

1. **Deploy Pipeline** — Creates a Resource Group, then deploys a Linux Virtual Machine and a Storage Account using an ARM template from GitHub
2. **List Pipeline** — Lists all resources inside the Resource Group to confirm deployment
3. **Delete Pipeline** — Deletes all resources inside the Resource Group to clean up

---

## Pre-Requisites

Before starting this task, make sure you have completed:
- Task 7: Run a Pipeline Using the Service Connection

Have the following ready from your Lab Environment Page:

| Item | Where to Find It |
|---|---|
| Azure DevOps Organization URL | Lab Environment Page |
| Azure DevOps Username | Lab Environment Page |
| Azure DevOps Password | Lab Environment Page |
| Azure Subscription ID | Lab Environment Page |
| GitHub Repository URL (containing ARM template) | Lab Environment Page |
| Service Connection Name | `azure-sp-connection` (created in Task 7) |

---

## Quick Background: What You Are Deploying

You will be using an **ARM (Azure Resource Manager) template** stored in a GitHub repository. ARM templates are JSON files that describe the Azure resources you want to create — including their configuration, SKU, and relationships.

| Resource | Type | Purpose |
|---|---|---|
| Resource Group | `Microsoft.Resources/resourceGroups` | Logical container for all resources |
| Virtual Machine | `Microsoft.Compute/virtualMachines` | Linux VM (Standard_B1s) |
| Storage Account | `Microsoft.Storage/storageAccounts` | General purpose v2 storage |

The ARM template will be fetched directly from your GitHub repository URL during the pipeline run — no manual download is needed.

---

## Part A: Prepare the ARM Template in GitHub

---

### Step 1: Confirm the ARM Template Exists in Your GitHub Repository

Before building the pipeline, verify that your GitHub repository contains an ARM template file. The template should be in your repo at a path like:

```
/templates/azuredeploy.json
```

> **Note:** If your ARM template file is at a different path or filename, note it down — you will need to replace the path in the pipeline script later.

Your ARM template should define at minimum:
- A **Virtual Machine** resource (with its NIC, VNet, Subnet, Public IP, and OS Disk)
- A **Storage Account** resource

---

### Step 2: Get the Raw GitHub URL of Your ARM Template

1. Open your GitHub repository in a browser
2. Navigate to the ARM template file (e.g., `templates/azuredeploy.json`)
3. Click the **Raw** button at the top right of the file view
4. Copy the URL from the browser address bar — it will look like:

```
https://raw.githubusercontent.com/YOUR-GITHUB-USERNAME/YOUR-REPO-NAME/main/templates/azuredeploy.json
```

5. Save this URL — you will use it in the pipeline scripts below

---

> [IMAGE PLACEHOLDER - Screenshot of the GitHub repository showing the ARM template file open with the Raw button highlighted at the top right]

---

## Part B: Pipeline 1 — Deploy Resources from ARM Template

---

### Step 3: Log In to Azure DevOps

1. Open a browser and go to the Azure DevOps Organization URL from your Lab Details Page
2. Enter your Azure DevOps Username and Password
3. Open your project (e.g., `contoso-lab-john`)

---

### Step 4: Navigate to Repos and Open the Pipeline File

1. In the left sidebar, click **Repos**
2. Find and click on `azure-pipeline.yml`
3. Click the **Edit** button at the top right to enter edit mode

---

> [IMAGE PLACEHOLDER - Screenshot of the Repos file listing with azure-pipeline.yml visible and the Edit button highlighted]

---

### Step 5: Replace the Pipeline Content with the Deploy Script

1. Select all existing content in the editor (press `Ctrl+A`) and delete it
2. Paste the following YAML exactly as shown

> **Important:** Replace the following placeholders with your actual values:
> - `YOUR-RESOURCE-GROUP-NAME` — e.g., `rg-participant-john`
> - `YOUR-LOCATION` — e.g., `eastus`
> - `YOUR-RAW-GITHUB-URL` — the raw URL you copied in Step 2
> - `YOUR-VM-NAME` — the name you want to give your VM
> - `YOUR-STORAGE-ACCOUNT-NAME` — must be globally unique, lowercase, 3–24 characters

```yaml
trigger: none

pool:
  name: Default

steps:
- task: AzureCLI@2
  displayName: 'Create Resource Group'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Step 1: Creating Resource Group"
      echo "============================================"
      az group create \
        --name YOUR-RESOURCE-GROUP-NAME \
        --location YOUR-LOCATION
      echo ""
      echo "Resource Group created successfully."

- task: AzureCLI@2
  displayName: 'Deploy ARM Template from GitHub'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Step 2: Deploying ARM Template from GitHub"
      echo "============================================"
      echo "Fetching template from GitHub and starting deployment..."
      echo ""
      az deployment group create \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --name arm-deployment-$(date +%Y%m%d%H%M%S) \
        --template-uri YOUR-RAW-GITHUB-URL \
        --parameters \
          vmName=YOUR-VM-NAME \
          storageAccountName=YOUR-STORAGE-ACCOUNT-NAME \
        --verbose
      echo ""
      echo "============================================"
      echo "ARM Template deployment completed."
      echo "============================================"

- task: AzureCLI@2
  displayName: 'Verify Deployed Resources'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Step 3: Verifying Deployed Resources"
      echo "============================================"
      echo ""
      echo "Resources in Resource Group: YOUR-RESOURCE-GROUP-NAME"
      echo "--------------------------------------------"
      az resource list \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --output table
      echo ""
      echo "VM Power State:"
      az vm show \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --name YOUR-VM-NAME \
        --show-details \
        --query "powerState" \
        --output tsv
      echo ""
      echo "Storage Account Status:"
      az storage account show \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --name YOUR-STORAGE-ACCOUNT-NAME \
        --query "statusOfPrimary" \
        --output tsv
      echo ""
      echo "All resources verified successfully."
```

---

> [IMAGE PLACEHOLDER - Screenshot of the editor with the Deploy ARM Template YAML content pasted in, with placeholders replaced and visible]

---

### Step 6: Understand What the Deploy Pipeline Does

| Step | Command | What It Does |
|---|---|---|
| Step 1 | `az group create` | Creates a new Resource Group in the specified Azure region |
| Step 2 | `az deployment group create` | Fetches the ARM template from GitHub and deploys all resources defined in it |
| `--template-uri` | GitHub Raw URL | Points directly to the ARM JSON file — no download needed |
| `--parameters` | Key=Value pairs | Passes VM name and Storage Account name into the ARM template |
| Step 3 | `az resource list` | Lists all resources deployed in the Resource Group in a table |
| `az vm show --show-details` | Checks live VM power state | Confirms the VM is running post-deployment |
| `az storage account show` | Checks storage account status | Confirms storage is available |

---

### Step 7: Commit the Deploy Pipeline File

1. Click the **Commit** button at the top right of the editor
2. In the commit message field, type:

```
Add pipeline to deploy RG, VM, and Storage Account from ARM template
```

3. Leave the branch as `main`
4. Select **Commit directly to the main branch**
5. Click the **Commit** button inside the dialog

---

> [IMAGE PLACEHOLDER - Screenshot of the Commit dialog with the commit message about ARM template deployment and the Commit button visible]

---

### Step 8: Run the Deploy Pipeline

1. In the left sidebar, click **Pipelines**
2. Click on your pipeline name
3. Click **Run pipeline** at the top right
4. Leave all settings as default
5. Click **Run**

---

> [IMAGE PLACEHOLDER - Screenshot of the Pipelines section with the Run pipeline button highlighted]

---

### Step 9: Watch the Deploy Pipeline Run

1. Click on the **Job** link to see detailed logs
2. The pipeline has **three steps** — watch each one complete in sequence
3. Deployment will take **5–10 minutes** as Azure provisions the VM, networking components, and storage account
4. Watch for these log messages:
   - `"Resource Group created successfully."`
   - `"ARM Template deployment completed."`
   - A resource table showing your VM and Storage Account
   - `"VM running"` and `"available"` in the verification step

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline job logs showing the three steps running in sequence, with the ARM deployment output visible]

---

### Step 10: Read the Final Output of the Deploy Pipeline

After the pipeline completes successfully, the logs should show output similar to:

```
Resource Group created successfully.

ARM Template deployment completed.

Resources in Resource Group: rg-participant-john
--------------------------------------------
Name                    ResourceGroup         Location    Type
----------------------  --------------------  ----------  ------------------------------------
participant-john-vm     rg-participant-john   eastus      Microsoft.Compute/virtualMachines
participantjohnsa       rg-participant-john   eastus      Microsoft.Storage/storageAccounts
...

VM Power State:
VM running

Storage Account Status:
available

All resources verified successfully.
```

---

> [IMAGE PLACEHOLDER - Screenshot of the completed pipeline log showing the resource table and the VM running and storage account available status]

---

### Step 11: Confirm Resources in the Azure Portal

1. Open the Azure Portal at `https://portal.azure.com`
2. Search for **Resource Groups** and click on your Resource Group
3. Confirm you can see both the VM and the Storage Account listed in the resources

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure Portal Resource Group overview showing the Virtual Machine and Storage Account in the resources list]

---

## Part C: Pipeline 2 — List Resources in the Resource Group

---

### Step 12: Open the Pipeline File for Editing Again

1. In Azure DevOps, click **Repos** in the left sidebar
2. Click on `azure-pipeline.yml`
3. Click the **Edit** button

---

### Step 13: Replace the Pipeline Content with the List Resources Script

1. Select all content (`Ctrl+A`) and delete it
2. Paste the following YAML:

> **Important:** Replace `YOUR-RESOURCE-GROUP-NAME` with your actual Resource Group name.

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:
- task: AzureCLI@2
  displayName: 'List All Resources in Resource Group'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Listing All Resources in Resource Group"
      echo "============================================"
      echo ""

      echo "--- Resource Group Details ---"
      az group show \
        --name YOUR-RESOURCE-GROUP-NAME \
        --output table
      echo ""

      echo "--- All Resources (Table View) ---"
      az resource list \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --output table
      echo ""

      echo "--- Virtual Machine Details ---"
      az vm list \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --show-details \
        --output table
      echo ""

      echo "--- Storage Account Details ---"
      az storage account list \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --output table
      echo ""

      echo "--- Resource Count Summary ---"
      TOTAL=$(az resource list \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --query "length(@)" \
        --output tsv)
      echo "Total resources in Resource Group: $TOTAL"
      echo ""
      echo "Listing complete."
```

---

> [IMAGE PLACEHOLDER - Screenshot of the editor with the List Resources YAML content pasted in and the resource group placeholder replaced]

---

### Step 14: Understand What the List Pipeline Does

| Command | What It Does |
|---|---|
| `az group show` | Displays details about the Resource Group itself (location, status) |
| `az resource list --output table` | Lists every resource in the RG in a clean table with Name, Type, and Location |
| `az vm list --show-details` | Shows VM-specific details including power state, OS type, and size |
| `az storage account list` | Shows storage account details including SKU and replication type |
| `az resource list --query "length(@)"` | Counts and displays the total number of resources |

---

### Step 15: Commit and Run the List Pipeline

1. Click **Commit** and enter the message:

```
Add pipeline to list all resources in the Resource Group
```

2. Commit directly to `main`
3. Go to **Pipelines**, click your pipeline, and click **Run pipeline**
4. Click **Run** to start

---

> [IMAGE PLACEHOLDER - Screenshot of the Commit dialog with the list pipeline commit message visible]

---

### Step 16: Read the Final Output of the List Pipeline

After the pipeline completes, the logs should show output similar to:

```
--- Resource Group Details ---
Name                   Location    Status
---------------------  ----------  ---------
rg-participant-john    eastus      Succeeded

--- All Resources (Table View) ---
Name                    ResourceGroup         Location    Type
----------------------  --------------------  ----------  ------------------------------------
participant-john-vm     rg-participant-john   eastus      Microsoft.Compute/virtualMachines
participant-john-nic    rg-participant-john   eastus      Microsoft.Network/networkInterfaces
participant-john-pip    rg-participant-john   eastus      Microsoft.Network/publicIPAddresses
participant-john-vnet   rg-participant-john   eastus      Microsoft.Network/virtualNetworks
participant-john-nsg    rg-participant-john   eastus      Microsoft.Network/networkSecurityGroups
participantjohnsa       rg-participant-john   eastus      Microsoft.Storage/storageAccounts

--- Virtual Machine Details ---
Name                   ResourceGroup         PowerState    OsType
---------------------  --------------------  ------------  --------
participant-john-vm    rg-participant-john   VM running    Linux

--- Storage Account Details ---
Name               ResourceGroup         Location    Sku
-----------------  --------------------  ----------  -----------
participantjohnsa  rg-participant-john   eastus      Standard_LRS

--- Resource Count Summary ---
Total resources in Resource Group: 6

Listing complete.
```

---

> [IMAGE PLACEHOLDER - Screenshot of the completed List pipeline log showing all resources listed in table format with the total count at the bottom]

---

## Part D: Pipeline 3 — Delete All Resources in the Resource Group

> ⚠️ **Warning:** This pipeline **permanently deletes** the Resource Group and all resources inside it. This action cannot be undone. Run this only when you are ready to clean up.

---

### Step 17: Open the Pipeline File for Editing Again

1. In Azure DevOps, click **Repos**
2. Click on `azure-pipeline.yml`
3. Click **Edit**

---

### Step 18: Replace the Pipeline Content with the Delete Script

1. Select all content (`Ctrl+A`) and delete it
2. Paste the following YAML:

> **Important:** Replace `YOUR-RESOURCE-GROUP-NAME` with your actual Resource Group name.

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:
- task: AzureCLI@2
  displayName: 'List Resources Before Deletion'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Step 1: Resources to be Deleted"
      echo "============================================"
      echo ""
      echo "The following resources will be permanently deleted:"
      echo ""
      az resource list \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --output table
      echo ""
      TOTAL=$(az resource list \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --query "length(@)" \
        --output tsv)
      echo "Total resources scheduled for deletion: $TOTAL"
      echo ""

- task: AzureCLI@2
  displayName: 'Delete All Resources in Resource Group'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Step 2: Deleting Resource Group and All Resources"
      echo "============================================"
      echo ""
      echo "Sending delete command for Resource Group: YOUR-RESOURCE-GROUP-NAME"
      echo "This will delete all resources inside the group..."
      echo ""
      az group delete \
        --name YOUR-RESOURCE-GROUP-NAME \
        --yes \
        --no-wait
      echo ""
      echo "Delete command sent. Waiting for deletion to complete..."
      echo ""

- task: AzureCLI@2
  displayName: 'Confirm Resource Group Deletion'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Step 3: Confirming Deletion"
      echo "============================================"
      echo ""
      echo "Checking if Resource Group still exists..."
      echo ""
      RG_EXISTS=$(az group exists --name YOUR-RESOURCE-GROUP-NAME)

      if [ "$RG_EXISTS" = "true" ]; then
        echo "Resource Group is still being deleted. This may take a few more minutes."
        echo "You can verify final deletion in the Azure Portal."
      else
        echo "Resource Group 'YOUR-RESOURCE-GROUP-NAME' has been successfully deleted."
        echo "All resources inside it have been removed."
      fi
      echo ""
      echo "Cleanup complete."
```

---

> [IMAGE PLACEHOLDER - Screenshot of the editor with the Delete pipeline YAML content pasted in and resource group placeholder replaced]

---

### Step 19: Understand What the Delete Pipeline Does

| Step | Command | What It Does |
|---|---|---|
| Step 1 | `az resource list` | Lists and counts all resources before deletion so you have a record |
| Step 2 | `az group delete --yes --no-wait` | Deletes the entire Resource Group and all resources inside it without prompting for confirmation |
| `--yes` | Skips the interactive confirmation prompt | Required in automated pipelines where no user is present to type "y" |
| `--no-wait` | Does not block the pipeline waiting for full deletion | The delete is started in Azure; the pipeline does not wait for it to fully complete |
| Step 3 | `az group exists` | Checks whether the Resource Group still exists and prints the appropriate status message |

---

### Step 20: Commit the Delete Pipeline File

1. Click **Commit** and enter the message:

```
Add pipeline to delete all resources in the Resource Group
```

2. Commit directly to `main`

---

> [IMAGE PLACEHOLDER - Screenshot of the Commit dialog with the delete pipeline commit message visible]

---

### Step 21: Run the Delete Pipeline

1. Go to **Pipelines** in the left sidebar
2. Click your pipeline name
3. Click **Run pipeline**
4. Leave all settings as default and click **Run**

---

> [IMAGE PLACEHOLDER - Screenshot of the Pipelines section with the Run pipeline button highlighted, ready to trigger the Delete pipeline]

---

### Step 22: Watch the Delete Pipeline Run

1. Click on the **Job** link to view detailed logs
2. The pipeline has **three steps** — all should complete quickly
3. Watch for these messages:
   - A resource table showing resources before deletion (Step 1)
   - `"Delete command sent."` (Step 2)
   - `"Resource Group has been successfully deleted."` or the in-progress message (Step 3)

> Note: Because `--no-wait` is used, the pipeline will complete before Azure finishes deleting all resources. The actual deletion may take an additional 2–5 minutes in the background.

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline job logs showing Step 1 listing resources, Step 2 sending the delete command, and Step 3 confirming deletion]

---

### Step 23: Verify Deletion in the Azure Portal

1. Switch to the Azure Portal browser tab
2. Search for **Resource Groups** and look for your Resource Group name
3. It should no longer appear in the list
4. If it still shows, wait 2–3 minutes and refresh the page — Azure deletes resources in the background

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure Portal Resource Groups list showing the participant's resource group is no longer present, confirming successful deletion]

---

## Part E: Review the Complete Pipeline Run History

---

### Step 24: Check All Three Pipeline Runs in Azure DevOps

1. In Azure DevOps, click **Pipelines** in the left sidebar
2. Click on your pipeline name
3. You will see the **Run History** showing all three pipeline runs:
   - Run 1: Deploy ARM Template (green Succeeded)
   - Run 2: List Resources (green Succeeded)
   - Run 3: Delete Resources (green Succeeded)
4. Click on each run to review its individual logs

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline Run History showing three successful runs with their commit message labels: Deploy, List, and Delete]

---

## Task Completion Checklist

```
[ ] Confirmed ARM template is present in GitHub repository
[ ] Copied the raw GitHub URL for the ARM template
[ ] Logged in to Azure DevOps
[ ] Opened and edited azure-pipeline.yml in Repos

--- Deploy Pipeline ---
[ ] Pasted Deploy pipeline YAML into the editor
[ ] Replaced all four occurrences of YOUR-RESOURCE-GROUP-NAME
[ ] Replaced YOUR-LOCATION with the correct Azure region
[ ] Replaced YOUR-RAW-GITHUB-URL with the actual raw GitHub URL
[ ] Replaced YOUR-VM-NAME and YOUR-STORAGE-ACCOUNT-NAME
[ ] Committed the Deploy pipeline with a descriptive commit message
[ ] Ran the Deploy pipeline and watched all three steps complete
[ ] Confirmed resource table and VM/Storage status in logs
[ ] Verified VM and Storage Account visible in Azure Portal

--- List Pipeline ---
[ ] Replaced pipeline content with List pipeline YAML
[ ] Replaced YOUR-RESOURCE-GROUP-NAME in all list commands
[ ] Committed the List pipeline with a descriptive commit message
[ ] Ran the List pipeline and confirmed all resources appear in output
[ ] Confirmed total resource count shown in logs

--- Delete Pipeline ---
[ ] Replaced pipeline content with Delete pipeline YAML
[ ] Replaced YOUR-RESOURCE-GROUP-NAME in all delete commands
[ ] Committed the Delete pipeline with a descriptive commit message
[ ] Ran the Delete pipeline and confirmed delete command was sent
[ ] Waited and verified Resource Group no longer exists in Azure Portal

--- Final Review ---
[ ] Reviewed pipeline Run History showing all three successful runs
[ ] Completed the Final Verification Summary table above
```

---

## Key Commands Reference

| Command | Purpose |
|---|---|
| `az group create` | Creates a new Resource Group |
| `az deployment group create --template-uri` | Deploys an ARM template from a URL (e.g., GitHub raw URL) |
| `az resource list --output table` | Lists all resources in a Resource Group |
| `az vm list --show-details` | Lists VMs with power state and OS details |
| `az storage account list` | Lists storage accounts with SKU and location |
| `az resource list --query "length(@)"` | Returns the count of resources in a group |
| `az group delete --yes --no-wait` | Deletes the Resource Group and all resources without prompting |
| `az group exists` | Returns `true` or `false` — checks if a Resource Group still exists |
