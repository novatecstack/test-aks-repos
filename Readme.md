# Build and deploy to Azure Kubernetes Service (AKS) with Azure Pipelines

## Prerequisites
   - An Azure account with an active subscription.
   - An Azure Resource Manager service connection in Azure DevOps Org.
   - A GitHub account.

## Step-01: Develop the application
   ```
   # The app folder has the sample application code and a Dockerfile which we will be using in this lab
   ```

## Step-02: Create the Azure Resources - Resource Group | ACR | AKS Cluster

### Step-2.1: Create the Azure Resource Group
   ```
   # Create a Resource Group
   az group create --name binapp-rg --location eastus
   ```
### Step-2.2: Create the Azure Container Registry
   ```
   # Create a Container Registry
   az acr create --resource-group binapp-rg --name bincontainerregistry --sku Basic
   ```

### Step-2.3: Create an AKS cluster
   ```
   # Create a Kubernetes cluster (https://learn.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest#az-aks-create)
   az aks create --resource-group binapp-rg --name binakscluster --node-count 1 --enable-addons monitoring --generate-ssh-keys --node-vm-size Standard_B2s
   ```

## Step-03: Sign-in and setup Azure DevOps Account

### Step-3.1: Create an `Azure DevOps Organization`
   - Navigate to https://dev.azure.com/
   - Enter the credentials in order to sign-in
   - Now, Create an Organization

### Step-3.2: Create a `Project` under Azure DevOps Organization
   - Select the Organization >>  Click on `New Project` button
   - Project Name: <project_name>
   - Description: <project_description>
   - Visibility: Private
   - Version control: Git
   - Work Item Process: Basic

### Step-3.3: Create an `Azure Resource Manager Service Connection`
   - Select the Project created in step-3.2 >> Project Settings >> Service Connections.
   - Click on `New Service Connection` button >> Azure Resource Manager >> Select `Workload identity federation (automatic)`.
   - If prompted, enter the Azure AD credentials >> Select the Subscription >> Select the Resource Group.
   - Enter the `Service Connection Name`: <service_connection_name>.
   - Under Security section, check the checkbox `Grant access permission to all pipelines`.
   - Click `Save` button.
     
### Step-3.4: Create the Pipeline
   - Sign in to your Azure DevOps organization and go to your project.
   - Go to Pipelines, and then select New pipeline.
   - Select GitHub/Azure Repos as the location of your source code. If prompted, enter your GitHub credentials. When you see the list of repositories, select your repository.
   - Select `Deploy to Azure Kubernetes Service`.
   - If you're prompted, select the subscription in which you created your registry and cluster.

