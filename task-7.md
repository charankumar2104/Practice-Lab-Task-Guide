# Task 7: Start and Stop the VM via Pipeline - Restore to Running State

**Difficulty:** Intermediate  
**Estimated Time:** 30-35 Minutes  
**Type:** Hands-On Change with Full Revert (You will stop the VM via pipeline and then start it again to restore the original running state)

---

## Objective

In this task, you will modify your existing YAML pipeline to send a **stop command** to your pre-deployed Linux Virtual Machine using Azure CLI. You will observe the VM changing its state in the Azure Portal. You will then update the pipeline again to **start the VM**, restoring it back to the Running state it was in before this task began.

---

## Pre-Requisites

Before starting this task, make sure you have completed:
- Task 5: Create a Service Connection in Azure DevOps
- Task 6: Run a Pipeline Using the Service Connection

Have the following ready from your Lab Environment Page:

| Item | Where to Find It |
|---|---|
| Azure DevOps Organization URL | Lab Environment Page |
| Azure DevOps Username | Lab Environment Page |
| Azure DevOps Password | Lab Environment Page |
| Your Resource Group Name | Lab Environment Page (e.g., rg-participant-john) |
| Your VM Name | Lab Environment Page or from Task 2 notes |
| Service Connection Name | azure-sp-connection (created in Task 8) |

---

## Quick Background: VM Power States in Azure

A Virtual Machine in Azure can be in several different power states:

| Power State | What It Means | Are You Billed for Compute? |
|---|---|---|
| Running | VM is fully on and operational | Yes |
| Stopping | VM is in the process of shutting down | Yes (briefly) |
| Stopped | VM was shut down from inside the OS but is still allocated | Yes |
| Deallocated | VM is fully stopped and compute resources released | No |
| Starting | VM is in the process of booting up | Yes (briefly) |

When you use the Azure CLI command `az vm stop`, Azure performs a **deallocate** — meaning the VM is stopped AND compute resources are released. This is different from just shutting down the OS.

In real projects, teams use automated pipelines to stop VMs at the end of the working day and start them again in the morning — this saves significant compute costs in non-production environments.

---

## Part A: Verify the VM Is Running Before You Begin

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

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure Portal login page]

---

### Step 2: Navigate to Your Resource Group

1. Click the Search Bar at the top of the Azure Portal home page
2. Type: Resource Groups
3. Click Resource Groups from the dropdown
4. Find and click your Resource Group name (e.g., rg-participant-john)

---

> [IMAGE PLACEHOLDER - Screenshot of the Resource Groups search result and the participant's RG highlighted in the list]

---

### Step 3: Open Your Linux Virtual Machine

1. Inside your Resource Group, look at the resources list
2. Find the resource where the Type column says Virtual machine
3. Click on the VM name to open it

---

> [IMAGE PLACEHOLDER - Screenshot of the Resource Group overview with the Virtual Machine resource highlighted in the resources list]

---

### Step 4: Confirm the VM is in Running State

1. On the VM Overview page, look at the top section
2. Find the field labeled Status
3. It should currently say Running
4. Note this down — this is the state you must restore at the end of this task

---

> [IMAGE PLACEHOLDER - Screenshot of the Virtual Machine Overview page with the Status field showing "Running" highlighted with a label]

---

## Part B: Modify the Pipeline to Stop the VM

---

### Step 5: Log In to Azure DevOps

1. Open a new browser tab
2. Go to the Azure DevOps Organization URL from your Lab Details Page
3. Enter your Azure DevOps Username and click Next
4. Enter your Password and click Sign in
5. Open your project (e.g., contoso-lab-john)

---

> [IMAGE PLACEHOLDER - Screenshot of the Azure DevOps project home page after login]

---

### Step 6: Navigate to Repos and Open the Pipeline File

1. In the left sidebar, click Repos
2. In the file listing, find and click on azure-pipeline.yml
3. The file will open showing the YAML content from Task 9

---

> [IMAGE PLACEHOLDER - Screenshot of the Repos file listing with azure-pipeline.yml visible and highlighted]

---

### Step 7: Open the File for Editing

1. With azure-pipeline.yml open and its content visible on screen
2. Look for the **Edit** button at the top right of the file viewer
3. Click **Edit**
4. The file will switch to edit mode and you can now modify the content

---

> [IMAGE PLACEHOLDER - Screenshot of the azure-pipeline.yml file view with the Edit button highlighted at the top right]

---

### Step 8: Replace the Pipeline Content with the Stop VM Script

1. Select all the existing content in the editor (press Ctrl+A to select all)
2. Delete the selected content
3. Type or paste the following YAML exactly as shown below

> Important: Replace YOUR-RESOURCE-GROUP-NAME with your actual Resource Group name and YOUR-VM-NAME with your actual VM name from your Lab Details Page.

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:
- task: AzureCLI@2
  displayName: 'Stop the Virtual Machine'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Stopping the Virtual Machine"
      echo "============================================"
      az vm stop \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --name YOUR-VM-NAME
      echo ""
      echo "Stop command sent. Checking VM status..."
      echo ""
      az vm show \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --name YOUR-VM-NAME \
        --show-details \
        --query "powerState" \
        --output tsv
      echo ""
      echo "VM stop operation completed."
```

4. Confirm that:
   - Both occurrences of YOUR-RESOURCE-GROUP-NAME are replaced with your actual RG name
   - Both occurrences of YOUR-VM-NAME are replaced with your actual VM name

---

> [IMAGE PLACEHOLDER - Screenshot of the editor with the Stop VM YAML content pasted in, with the resource group and VM name fields replaced and visible]

---

### Step 9: Understand What the Stop Pipeline Does

Before committing, understand each part of the new script:

| Command / Section | What It Does |
|---|---|
| `az vm stop` | Sends a stop and deallocate command to the specified VM |
| `--resource-group` | Specifies which Resource Group the VM lives in |
| `--name` | Specifies the name of the VM to stop |
| `az vm show` | Retrieves the current details of the VM after the stop command |
| `--show-details` | Includes live power state information in the output |
| `--query "powerState"` | Filters the output to show only the power state field |
| `--output tsv` | Outputs the result as plain text (Tab Separated Values) |

---

### Step 10: Commit the Updated Pipeline File

1. Click the **Commit** button at the top right of the editor
2. A commit dialog will appear
3. In the commit message field, type:

```
Update pipeline to stop the Virtual Machine
```

4. Leave the branch as main
5. Select "Commit directly to the main branch"
6. Click the **Commit** button inside the dialog

---

> [IMAGE PLACEHOLDER - Screenshot of the Commit dialog with the commit message "Update pipeline to stop the Virtual Machine" typed in and the Commit button visible]

---

### Step 11: Navigate to Pipelines and Run the Pipeline

1. In the left sidebar, click **Pipelines**
2. You will see your pipeline listed (it may be named after your repository or shown as "azure-pipeline.yml")
3. Click on the pipeline name to open it
4. Click the **Run pipeline** button at the top right

---

> [IMAGE PLACEHOLDER - Screenshot of the Pipelines section with the pipeline listed and the "Run pipeline" button highlighted at the top right]

---

### Step 12: Confirm the Run Settings and Start

1. A side panel will appear showing run options
2. Leave all settings as default
3. Click the **Run** button at the bottom of the panel to start the pipeline

---

> [IMAGE PLACEHOLDER - Screenshot of the "Run pipeline" side panel with default settings and the Run button at the bottom highlighted]

---

### Step 13: Watch the Stop Pipeline Run

1. You will be taken to the Pipeline Run page
2. The job will first show as Queued, then Running
3. Click on the Job link to watch the detailed logs
4. The pipeline will take 2-4 minutes to complete because stopping a VM takes time
5. Watch for these log messages to appear:
   - "Stopping the Virtual Machine"
   - "Stop command sent. Checking VM status..."
   - A final line showing the VM power state

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline job logs showing the "Stopping the Virtual Machine" message and the progress of the az vm stop command running]

---

### Step 14: Read the Final Output of the Stop Pipeline

1. After the pipeline completes successfully, look at the final lines of the log
2. You should see output similar to:

```
Stopping the Virtual Machine
Stop command sent. Checking VM status...

VM deallocated

VM stop operation completed.
```

3. The text **VM deallocated** confirms the VM has been fully stopped and its compute resources have been released

---

> [IMAGE PLACEHOLDER - Screenshot of the completed pipeline log showing the final output including "VM deallocated" text]

---

### Step 15: Verify the VM State in the Azure Portal

1. Switch back to the Azure Portal browser tab
2. Navigate to your Resource Group and click on your Virtual Machine
3. On the VM Overview page, look at the **Status** field
4. It should now show **Stopped (deallocated)**

> Note: If the portal still shows Running, wait 30 seconds and click the Refresh button at the top of the VM overview page. The portal may take a moment to reflect the latest state.

---

> [IMAGE PLACEHOLDER - Screenshot of the Virtual Machine Overview page in the Azure Portal showing Status = "Stopped (deallocated)" highlighted]

---

## Part C: Modify the Pipeline to Start the VM (Revert to Default)

---

### Step 16: Go Back to Azure DevOps and Open the Pipeline File for Editing

1. Switch back to the Azure DevOps browser tab
2. In the left sidebar, click **Repos**
3. Click on **azure-pipeline.yml** to open it
4. Click the **Edit** button to enter edit mode

---

> [IMAGE PLACEHOLDER - Screenshot of the Repos section with azure-pipeline.yml open and the Edit button highlighted]

---

### Step 17: Replace the Pipeline Content with the Start VM Script

1. Select all content in the editor (press Ctrl+A)
2. Delete the selected content
3. Type or paste the following YAML exactly as shown below

> Important: Replace YOUR-RESOURCE-GROUP-NAME and YOUR-VM-NAME with your actual values, exactly as you did in Step 8.

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:
- task: AzureCLI@2
  displayName: 'Start the Virtual Machine'
  inputs:
    azureSubscription: 'azure-sp-connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      echo "============================================"
      echo "Starting the Virtual Machine"
      echo "============================================"
      az vm start \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --name YOUR-VM-NAME
      echo ""
      echo "Start command sent. Checking VM status..."
      echo ""
      az vm show \
        --resource-group YOUR-RESOURCE-GROUP-NAME \
        --name YOUR-VM-NAME \
        --show-details \
        --query "powerState" \
        --output tsv
      echo ""
      echo "VM start operation completed."
```

---

> [IMAGE PLACEHOLDER - Screenshot of the editor with the Start VM YAML content pasted in, with resource group and VM name replaced]

---

### Step 18: Understand What the Start Pipeline Does

| Command / Section | What It Does |
|---|---|
| `az vm start` | Sends a start command to the specified VM — allocates compute and boots the OS |
| `--resource-group` | Specifies which Resource Group the VM lives in |
| `--name` | Specifies the name of the VM to start |
| `az vm show` | Retrieves the current details of the VM after the start command |
| `--query "powerState"` | Filters the output to show only the power state field |
| `--output tsv` | Outputs the result as plain text |

---

### Step 19: Commit the Start VM Pipeline File

1. Click the **Commit** button at the top right of the editor
2. In the commit message field, type:

```
Update pipeline to start the Virtual Machine and restore running state
```

3. Leave the branch as main
4. Select "Commit directly to the main branch"
5. Click the **Commit** button inside the dialog

---

> [IMAGE PLACEHOLDER - Screenshot of the Commit dialog with the commit message about starting the VM and the Commit button visible]

---

### Step 20: Navigate to Pipelines and Run the Start Pipeline

1. In the left sidebar, click **Pipelines**
2. Click on your pipeline name
3. Click the **Run pipeline** button at the top right
4. In the side panel, leave all settings as default
5. Click **Run**

---

> [IMAGE PLACEHOLDER - Screenshot of the Pipelines section with the Run pipeline button highlighted and the run options panel visible]

---

### Step 21: Watch the Start Pipeline Run

1. You will be taken to the Pipeline Run page
2. Click on the **Job** link to see detailed logs
3. The pipeline will take 2-4 minutes because starting a VM (booting the OS) takes time
4. Watch for these messages in the logs:
   - "Starting the Virtual Machine"
   - "Start command sent. Checking VM status..."
   - The final power state output

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline job logs showing the "Starting the Virtual Machine" message and the progress of the az vm start command]

---

### Step 22: Read the Final Output of the Start Pipeline

1. After the pipeline completes, look at the final lines of the log
2. You should see output similar to:

```
Starting the Virtual Machine
Start command sent. Checking VM status...

VM running

VM start operation completed.
```

3. The text **VM running** confirms the VM has been fully started and is operational again

---

> [IMAGE PLACEHOLDER - Screenshot of the completed pipeline log showing the final output including "VM running" text and the Succeeded status]

---

### Step 23: Verify the VM Is Back to Running State in the Azure Portal

1. Switch back to the Azure Portal browser tab
2. Navigate to your Resource Group and click on your Virtual Machine
3. On the VM Overview page, look at the **Status** field
4. It should now show **Running**

> Note: If the portal still shows Stopped (deallocated), wait 30 seconds and click the Refresh button. The portal may take a moment to update.

---

> [IMAGE PLACEHOLDER - Screenshot of the Virtual Machine Overview page in the Azure Portal showing Status = "Running" — confirming the VM has been restored to its original state]

---

### Step 24: Compare the Pipeline Run History

1. Switch back to Azure DevOps
2. In the left sidebar, click **Pipelines**
3. Click on your pipeline name
4. You will see the **Run History** — a list of all times this pipeline was run
5. You should see at least two completed runs:
   - One run for the Stop operation
   - One run for the Start operation
6. Both should show a green Succeeded status
7. Click on each run to see its individual logs if you want to review them

---

> [IMAGE PLACEHOLDER - Screenshot of the pipeline Run History showing two successful runs — one labeled with the Stop commit message and one with the Start commit message]

---

### Step 25: Final Verification Summary

Confirm the following before marking this task complete:

| Check | Expected Result | Confirmed |
|---|---|---|
| VM Status in Azure Portal | Running | |
| Stop pipeline run status | Succeeded | |
| Start pipeline run status | Succeeded | |
| Stop pipeline log output | Shows "VM deallocated" | |
| Start pipeline log output | Shows "VM running" | |
| No extra resources created | Resource Group has the same resources as before | |

---

> [IMAGE PLACEHOLDER - Screenshot showing both the Azure Portal VM status as Running and the Azure DevOps pipeline run history side by side]

---

## Task Completion Checklist

Before marking this task as complete, confirm you have done all of the following:

```
[ ] Verified the VM was in Running state before starting
[ ] Logged in to Azure DevOps
[ ] Opened and edited azure-pipeline.yml in Repos
[ ] Replaced pipeline content with the Stop VM script
[ ] Replaced both resource group name and VM name placeholders correctly
[ ] Committed the Stop pipeline file with a descriptive commit message
[ ] Ran the Stop pipeline and watched the logs
[ ] Confirmed the log output showed "VM deallocated"
[ ] Verified in Azure Portal that VM status changed to "Stopped (deallocated)"
[ ] Went back to Repos and edited azure-pipeline.yml again
[ ] Replaced pipeline content with the Start VM script
[ ] Replaced both resource group name and VM name placeholders correctly
[ ] Committed the Start pipeline file with a descriptive commit message
[ ] Ran the Start pipeline and watched the logs
[ ] Confirmed the log output showed "VM running"
[ ] Verified in Azure Portal that VM status returned to "Running"
[ ] Reviewed the pipeline Run History and confirmed both runs succeeded
```
---