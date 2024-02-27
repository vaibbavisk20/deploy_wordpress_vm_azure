# Deploy a WordPress VM to Azure Action

## Description

This GitHub Action automates the deployment of a WordPress application on an Azure Virtual Machine (VM). The action provisions the VM and deploys the necessary components using an ARM template and parameter file.

**This documentation is for v1 of vaibbavisk20/deploy_wordpress_vm_azure**

## Inputs

Please install the Azure OIDC app from the GitHub Marketplace to populate the below inputs as secrets in your repo

- **client-id** (required): Client ID used for Azure login.
- **tenant-id** (required): Tenant ID used for Azure login.
- **subscription-id** (required): Azure subscription ID used with the `az login`.
- **resource-group-name** (required): Resource group where Azure resources will be deployed.

Set these values as secrets on your repo where the workflow runs

- **admin-username** (required): Admin username to log in to the VM.
  
  Username must only contain letters, numbers, hyphens, and underscores and may not start with a hyphen or number.
  Usernames must not include reserved words.
  The value is in between 1 and 64 characters long.
  
- **admin-password** (required): Admin password to log in to the VM.

  Password must have 3 of the following: 1 lowercase character, 1 uppercase character, 1 number, and 1 special character.
  The value must be between 12 and 72 characters long.

## Usage

Create this workflow in your repo on this path: `.github/workflows/workflow_file.yml`

```yaml
name: Workflow to deploy WordPress on Azure

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:

permissions:
  id-token: write
  contents: read

jobs:
  deploy-resources-to-azure:
    runs-on: ubuntu-latest
    steps:
        - name: Checkout main
          uses: actions/checkout@v3
          
        - name: Deploy a WordPress VM to Azure action
          uses: vaibbavisk20/deploy_wordpress_vm_azure@v1
          with:
            client-id: ${{ secrets.AZURE_CLIENT_ID }}
            tenant-id: ${{ secrets.AZURE_TENANT_ID }}
            subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
            resource-group-name: ${{secrets.AZURE_RG}}
            admin-username: ${{secrets.ADMIN_USERNAME}}
            admin-password: ${{secrets.ADMIN_PASSWORD}}
```
## Output

The action creates a VM which can be viewed on portal.azure.com and provides information about the deployed VM, including the virtual machine name, location, and VM size.
        

