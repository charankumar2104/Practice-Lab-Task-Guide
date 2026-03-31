# Task 7: Run a Pipeline Using the Service Connection

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
- Task 6: Create a Service Connection in Azure DevOps

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

<img src="./images/Screenshot 2026-03-31 090612.png">

---

### Step 2: Open Your Project

1. After signing in, you will land on the Azure DevOps organization home page
2. You will see your project listed (e.g., contoso-lab-john)
3. Click on your project name to open it

---

<img src="./images/Screenshot 2026-03-31 090916.png">

---

### Step 3: Navigate to Repos

1. On the left sidebar of your project, look for the navigation menu
2. Click on **Repos**
3. The Repos section will open — this is where your code and pipeline files are stored

---

<img src="./images/Screenshot 2026-03-31 104208.png">

---

### Step 4: Initialize the Repository

1. If the repository is empty, you will see a page that says "Initialize your repository" or "This repository is empty"
2. Scroll down and find the **Initialize** button
3. Make sure the option to add a README is checked
4. Click the **Initialize** button

> Note: If the repository already has files in it, skip this step and proceed directly to Step 5.

---

<img src="./images/Screenshot 2026-03-31 104259.png">

---

### Step 5: Confirm the Repository Is Initialized

1. After initializing, you will see the repository with at least one file — usually a README.md
2. The file tree will be visible on the left
3. You are now ready to create a new pipeline file

---

<img src="./images/Screenshot 2026-03-31 105309.png">

---

### Step 6: Create a New File in the Repository

1. On the Repos page, look at the top-right area near the file listing
2. Click on the three dots (...) or the **+ Add** button/dropdown near the top of the file listing area
3. From the dropdown, select **New file**

---

<img src="./images/Screenshot 2026-03-31 105358.png">

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

<img src="./images/Screenshot 2026-03-31 105520.png">

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
  name: Default

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
      az resource list --resource-group ODL-azure-Contoso-2157974 --output table
```

5. In the last line, replace `YOUR-RESOURCE-GROUP-NAME` with your actual Resource Group name from the Lab Details Page
   - Example: if your RG is `ODL-azure-Contoso-2157974`, the line should read:
   - `az resource list --resource-group ODL-azure-Contoso-2157974 --output table`

---

<img src="./images/Screenshot 2026-03-31 105848.png">

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

<img src="./images/Screenshot 2026-03-31 110000.png">

---

### Step 11: Confirm the File Was Committed

1. After committing, you will be taken back to the Repos file listing
2. You should now see two files in your repository:
   - README.md
   - azure-pipeline.yml
3. Click on **azure-pipeline.yml** to confirm its contents look correct

---

<img src="./images/Screenshot 2026-03-31 110038.png">

---

### Step 12: Create a PAT token
1. Navigate to the User settings from the top right corner.
---
<img src="./images/Screenshot 2026-03-31 120318.png">


2. Select the option Personal access token.
---
<img src="./images/Screenshot 2026-03-31 120527.png">
---

3. Click on **+ New Token**
4. Provide the name as `agent-token`.
5. At scope provide the full access.
---

<img src="./images/Screenshot 2026-03-31 120928.png">

---

6. Make sure that after creating the token copy the token and store it some where on notepad.'


> **Note:** If the token missed you can regenerate the token from the same PAT page it self.

### Step 13: Connect with the VM
1. Get on to the Azure portal page.
2. Search for the VM.
3. Enter into the VM that is present there.
4. Check weather the VM is in running or else stop state.
5. If the VM is in stop state just try to start it.
6. Later access the VM from the terminal command using:
``` bash
ssh <username>@<DNSName>
```
7. Map the details of the command like username with your VM username and DNSName with your VM DNSName from Environment page.

---

<img src="./images/Screenshot 2026-03-31 124313.png">

---
8. Enter `yes` after clicking enter.
9. Enter the password that you see in the environment page.

>**Note:**You are unable to see the password while entering so carefully enter the password.

---

### Step 14: Updating the packages of the Vm

1. Run the below command to update the packages.
``` bash
sudo apt update
```

### Step 15: Creating the default runner.
1. Navigate to the azure devops portal by searching  `Azure Devops Organization` in the azure portal.
2. Click on the organization that is available and enter into the project as well.
3. At bottom left bottom search for the `Project Settings`. 
4. Select the `Agent pools` under the `Pipelines` section.
--- 

<img src="./images/Screenshot 2026-03-31 125721.png">

---
5. Click on the Default under Agent pools.

### Step 16: Adding the agent to the VM.
1. Click on the `Agents` tab.
2. Click on `Add Agent`. 
---

<img src="./images/Screenshot 2026-03-31 130049.png">

---

3. After that you will see a new tab select the `Linux` from the tab.

4. Click on the copy icon after the Download option. 

---

<img src="./images/Screenshot 2026-03-31 130315.png">

---

### Step 17: Setting up the agent configurations to the VM.
1. In the Virtual machine run below command.
``` bash
wget <link you copied>
```
  - Example: 
   wget https://download.agent.dev.azure.com/agent/4.271.0/vsts-agent-linux-x64-4.271.0.tar.gz

2. List the files and you will be some kind of file like `vsts-agent-linux-x64-4.271.0.tar.gz`.
3. Now run the command to decompress the file.
``` bash
tar -xf <compressed folder name>
```
  - Example: tar -xf vsts-agent-linux-x64-4.271.0.tar.gz
4. Now list out all the files and you can see files like: config.sh, run.sh, etc.
5. Run the below command in the virtual machine
``` bash
./config.sh
```
  - Enter (Y/N) Accept the Team Explorer Everywhere license agreement now? (press enter for N) > `Type y and enter`.
  - Enter server URL > `Enter the azure devops page URL only with till the organiation name`.
    - Example: `https://dev.azure.com/odluser2157974`
  - Enter authentication type (press enter for PAT) > `Press enter`
  - Enter personal access token > `Enter the PAT token that you have created in Step 12`.
  - Enter agent pool (press enter for default) > `press enter for default`.
  - Enter agent name (press enter for demo-vm) > `Press enter for default`.
  - Enter work folder (press enter for _work) > `Press enter for Default`.
---

<img src="./images/Screenshot 2026-03-31 132310.png">

---

6. Now you can see an agent created in the agent pool page with the offline status as below.
---

<img src="./images/Screenshot 2026-03-31 132602.png">

---
7. To make the agent to be in online run the below command and check the status in the agent pools.
``` bash
./run.sh
```
---
<img src="./images/Screenshot 2026-03-31 132813.png">

---
8. Now you can see the agent in the online state.

---

### Step 18: Navigate to Pipelines

1. In the left sidebar, click on **Pipelines**
2. The Pipelines section will open
3. If no pipelines exist yet, you will see a "Create Pipeline" prompt or an empty list
4. Click **New pipeline** or **Create Pipeline**

---

<img src="./images/Screenshot 2026-03-31 110158.png">

---

### Step 19: Select the Code Source

1. A page titled "Where is your code?" will appear
2. You will see several source options:
   - Azure Repos Git
   - GitHub
   - Bitbucket Cloud
   - Other Git
3. Click on **Azure Repos Git** since your YAML file is stored in the Azure Repos repository you just edited

---

<img src="./images/Screenshot 2026-03-31 110339.png">

---

### Step 20: Select Your Repository

1. A list of repositories in your project will appear
2. Click on your repository name (it will match your project name, e.g., contoso-lab-john)

---

<img src="./images/Screenshot 2026-03-31 110250.png">

---

### Step 21: Select the Existing YAML File

1. The next page will ask "Configure your pipeline"
2. You will see several options:
   - Starter pipeline
   - Existing Azure Pipelines YAML file
   - Other options
3. Click on **Existing Azure Pipelines YAML file**

---

<img src="./images/Screenshot 2026-03-31 110434.png">

---

### Step 22: Select the azure-pipeline.yml File

1. A panel will slide open on the right
2. In the **Branch** dropdown, make sure **main** (or master) is selected
3. In the **Path** dropdown, select **/azure-pipeline.yml**
4. Click the **Continue** button

---

<img src="./images/Screenshot 2026-03-31 110508.png">

---

### Step 23: Review the Pipeline YAML Before Running

1. Azure DevOps will show you a preview of your YAML file
2. Read through it and confirm:
   - `azureSubscription` says `azure-sp-connection`
   - The resource group name in the last line matches your actual RG name
3. If everything looks correct, click the **Run** button at the top right

---

<img src="./images/Screenshot 2026-03-31 110715.png">

---

### Step 24: Watch the Pipeline Run in Real Time

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

<img src="./images/Screenshot 2026-03-31 133612.png">

---

### Step 25: Permit the pipeline to run
1. Click on the view or else job that shown in the stages section.
---

<img src="./images/Screenshot 2026-03-31 133703.png">

---
2. Click on the permit buttom once below page is popped up.

---

<img src="./images/Screenshot 2026-03-31 133721.png">

---

### Step 26: Read the Pipeline Output Logs

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

<img src="./images/Screenshot 2026-03-31 143518.png">

---

### Step 27: Confirm the Pipeline Run Status Is "Succeeded"

1. Click the back arrow or the pipeline name breadcrumb to go back to the run summary page
2. The overall status should show **Succeeded** with a green checkmark
3. Note the run duration shown — this tells you how long the pipeline took from start to finish

---

<img src="./images/Screenshot 2026-03-31 143621.png">

---

### Step 21: Verify No Azure Resources Were Changed

1. Open a new browser tab
2. Go to `https://portal.azure.com`
3. Navigate to your Resource Group
4. Confirm the resources inside are exactly the same as before you ran the pipeline
5. No new resources have been created — the pipeline only read data, it did not create anything

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
