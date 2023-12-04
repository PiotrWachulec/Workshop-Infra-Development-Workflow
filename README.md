# \[Workshop\] Infra development workflow

Here are the materials for the workshop Infra development workflow. You will learn how to:

- preconfigure VS Code for the infrastructure development,
- write basic Bicep templates,
- do the deployment from Powershell,
- configure and run Powershell scripts from VS Code,
- (optional) configure basic deployment pipelines using GitHub Actions.

## Requirements

To install:

- Visual Studio Code
- Powershell Core 7+
- Azure Powershell
- git

Additionally:

- GitHub account
- access to the resource group on Azure with created and configured SPN with assigned Contributor role on the resource group level.

# Prerequisite concepts and additional materials

## Repository and security

Steps:
- Clone the repository: [github.com/PiotrWachulec/PureEvilRepo](https://github.com/PiotrWachulec/PureEvilRepo)
- Open the repository in VS Code and click 'Trust the authors'.

Materials:
- [https://code.visualstudio.com/docs/editor/workspace-trust](https://code.visualstudio.com/docs/editor/workspace-trust)

## Remote Repositories

If you need to open a repository from the internet, you can do it in VS Code without cloning that.

Steps:
- Install the extension: [https://marketplace.visualstudio.com/items?itemName=ms-vscode.remote-repositories](https://marketplace.visualstudio.com/items?itemName=ms-vscode.remote-repositories)
- Click on the green button on the left down corner and select `Open Remote Repository...`.
- Choose repository to open.

## Tasks

Tasks are a way to automate common actions in VS Code. They can be used to run commands, scripts, or other tasks. Tasks are defined in a file called `tasks.json` in the `.vscode` folder of your workspace. You can create multiple tasks in this file, each with its own name and configuration.

Tasks can be run from the command palette, the status bar, or the terminal. You can also configure tasks to run automatically when you open a workspace or when you save a file.

Steps:
- Install the extension: [https://marketplace.visualstudio.com/items?itemName=actboy168.tasks](https://marketplace.visualstudio.com/items?itemName=actboy168.tasks)
- Run from the command palette: `Create tasks.json file from template` and later select `Others`.
- Fill the `task.json` file with the example below from `./solutions/Tasks/.vscode/tasks.json` file.
- Copy scripts from `solutions/Tasks/scripts` to the `./scripts` folder.
- Add user-defined tasks by choosing from the command palette: `Tasks: Open User Tasks`.

Materials:
- [https://code.visualstudio.com/docs/editor/tasks](https://code.visualstudio.com/docs/editor/tasks)

## Az Predictor

The Az Predictor extension provides auto-completion for Azure PowerShell cmdlets and parameters. It uses machine learning to predict the most likely commands and parameters based on your previous usage patterns. You can also use it to search for cmdlets and parameters by name or description.

Steps:
- Install the Az Predictor like it is described in the instruction: [Intelligent context-aware command completion with Az Predictor](https://learn.microsoft.com/en-us/powershell/azure/az-predictor?view=azps-11.0.0)

## REST Client

The REST Client extension allows you to send HTTP requests and view the responses directly in VS Code. It supports all the major HTTP methods, including GET, POST, PUT, PATCH, DELETE, HEAD, and OPTIONS. You can also set headers and query parameters, and view the response body in JSON or XML format.

Steps:
- Install the extension: [https://marketplace.visualstudio.com/items?itemName=humao.rest-client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
- Create file `./api.http` and add the content from `./solutions/RESTClient/api.http` file.
- Run the request by clicking `Send Request` button.

Materials:
- [Postman's collection of different REST APIs that are completley public and do not require any authentication](https://documenter.getpostman.com/view/8854915/Szf7znEe)

## The `.git/info/exclude` file

The `.git/info/exclude` file is a Git configuration file that allows you to specify files and directories that Git should ignore when tracking changes to a project. It is similar to the `.gitignore` file, but it applies only to the local repository, rather than being committed and shared with other contributors.

The `exclude` file is useful when you want to exclude certain files or directories from being tracked by Git but don't want to add them to the `.gitignore` file because they are specific to your local environment or workflow. For example, you might use this file to exclude temporary files, log files, or build artifacts that are generated during the development process.

The syntax of the exclude file is the same as that of the `.gitignore` file. You can use wildcards and patterns to specify files or directories to exclude. Each pattern should be listed on a separate line.

Here's an example of a `.git/info/exclude` file that excludes all files with the extension `.log` and the build directory:

```bash
# exclude log files
*.log

# exclude build directory
build/
```

Note that any files or directories specified in the exclude file will still be present in your local repository, but they will be ignored by Git and will not be included in any commits or pushes to remote repositories.

## The `.local` folder

The `.local` folder is a concept where you have a dedicated folder for everything which you want to have in the project folder, but you don't want to commit to the repository. You can create a folder called `.local` and add it to the `.git/info/exclude`. Then you will be sure that everything that will land in this folder will not be committed to the repository. 

## Abbreviation examples for Azure resources

The naming convention for resources is always the subject of heated debate. In this document you have examples of abbreviations for Azure resources: [Abbreviation examples for Azure resources](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations).

There is also a tool which helps you to manage the naming convention: [Azure Naming Tool](https://github.com/mspnp/AzureNamingTool).

## More complex template designs

After this workshop, you should be able to write basic and later more complex Bicep templates. Then it is good to have a better overview of how they can be designed. In the [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/) you can find the document called: [Enterprise infrastructure as code using Bicep and Azure Container Registry](https://learn.microsoft.com/en-us/azure/architecture/guide/azure-resource-manager/advanced-templates/enterprise-infrastructure-bicep-container-registry) which describes the way of building reusable assets of templates.

## Splatting

In PowerShell, splatting is a technique that allows you to pass a collection of key-value pairs to a command or function as individual parameters. Instead of passing each parameter one by one, you can define a hashtable that contains the parameter names as keys and their corresponding values as values. This hashtable is then passed to the command or function using the @ symbol followed by the name of the hashtable.

Example:
```ps
$params = @{
  ComputerName = 'Server01'
  Credential   = (Get-Credential)
  Path         = 'C:\Temp\File.txt'
}

Get-Content @params
```

Splatting can be especially useful when you have a large number of parameters to pass to a command or function, or when you're building up a set of parameters dynamically. It can make your code easier to read and maintain since you can see all the parameters in one place rather than scattered throughout your code.

# Workshop

## \[E01\] Basic Bicep template

Defining resources is super easy in Bicep:

```
resource <symbolic-name> <type@api-version> (existing) = {
  name: name
  location: location
}
```

As you can see above you have to use `resource` keyword.

Optionally, you can add `existing` keyword if you are referring to the existing resource.

Later between the `{}` you have to fulfill the required properties.

Steps:
- Create folder `templates` in the root of the repository.
- Create file `template.bicep` in the `templates` folder.
- Add the below code to the `template.bicep` file.

```bicep
resource myResource 'Microsoft.Storage/storageAccounts@2021-02-01' = {
  name: 'myStorageAccount'
  location: 'eastus'
}
```

## [E02] Basic Powershell deployment

> The detailed instruction can be found in the documentation [Deploy resources with Bicep and Azure PowerShell](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deploy-powershell)

> Before running the deployment make sure that you logged into the Azure from Powershell (`Connect-AzAccount`) and set proper context (`Set-AzContext -SubscriptionId <SubscriptionID>`).

For running basic deployment you need to run the `New-AzResourceGroupDeployment` cmdlet.

```ps
$rgName = "infra-development-workflow-rg"
New-AzResourceGroupDeployment -Name "MyDeployment" -ResourceGroupName $rgName -TemplateFile './templates/template.bicep'
```

## \[E03\] Deployments in Azure Portal

When you are reviewing the resource group, you can find the 'Deployment' pane. It shows the deployment history, so you can check the detailed errors and so on. Due to this, it is important to make deployment names unique. More information can be found in the [documentation](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/deployment-history?tabs=azure-portal).

## \[E04\] Preparing PS script for the deployment

As we mentioned above, it would be good if the deployment names are unique. Let's prepare a script for running the deployments of the resources:

Steps:
- Create file `Deploy-Infrastructure.ps1` in the `scripts` folder.
- Fulfill the file with the below code.
```ps
param(
    [string]$ResourceGroupName
)

# Generate a timestamp to use in the deployment name
$timestamp = Get-Date -Format 'yyyyMMddHHmmss'

# Construct the deployment name using the prefix and timestamp
$deploymentName = "MyDeploment-$timestamp"

# Deploy the template using the Azure PowerShell module
New-AzResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rgName -TemplateFile './templates/template.bicep'
```

And later, the script can be called in that way:

```ps
$rgName = "infra-development-workflow-rg"
./scripts/Deploy-Infrastructure.ps1 -ResourceGroupName $rgName
```

For now, it looks like overengineering, but it can be helpful later.

## \[E05\] Running and debugging PS scripts in VS Code

> Demo

Materials:

- [Using Visual Studio Code for PowerShell Development](https://learn.microsoft.com/en-us/powershell/scripting/dev-cross-plat/vscode/using-vscode?view=powershell-7.3)
- [https://devblogs.microsoft.com/scripting/debugging-powershell-script-in-visual-studio-code-part-1/](https://devblogs.microsoft.com/scripting/debugging-powershell-script-in-visual-studio-code-part-1/)
- [VS Code debugging](https://code.visualstudio.com/docs/editor/debugging)

## \[E06\] Writing basic GitHub workflow (optional)

Assumption: you already have SPN with created secret and assigned roles.

First, create JSON like below:
```json
{
    "clientId": "<YOUR_CLIENT_ID>",
    "clientSecret": "<YOUR_CLIENT_SECRET>",
    "subscriptionId": "<YOUR_SUBSCRIPTION_ID>",
    "tenantId": "<YOUR_TENANT_ID>"
}
```

1. Open your GitHub repository and go to Settings.
1. Select Security > Secrets and variables > Actions.
1. Navigate to your GitHub repository.
1. Click on the "Settings" tab.
1. Click on "Secrets and variables / Actions" in the left-hand menu.
1. Click on "New repository secret".
1. Enter the name of the secret, such as `AZURE_CREDENTIALS`, and the value of the secret, which is the service principal credentials in JSON format. The JSON should include the `clientId`, `clientSecret`, `subscriptionId`, and `tenantId` fields.
1. Create `.github/workflows` directory
1. Add to the new file `deploy-resources.yaml`
1. Fulfill the new file with the below code

```yaml
name: Deploy Bicep Template

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Login to Azure
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        enable-AzPSSession: true
    
    - name: Set up Azure PowerShell module
      uses: azure/powershell@v1
      with:
        azPSVersion: latest
        inlineScript: |
          ./scripts/Deploy-Resources.ps1 -ResourceGroupName "bicep-workshop-test"
```

After that, commit changes and push the repository. The pipeline should be triggered automatically and the Bicep template will be deployed.

Materials: 
- [GitHub Action for Azure Login](https://github.com/marketplace/actions/azure-login)
- [Quickstart: Deploy Bicep files by using GitHub Actions](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deploy-github-actions)
- [GitHub action for Azure PowerShell](https://github.com/marketplace/actions/azure-powershell-action)
- [Quickstart for GitHub Actions](https://docs.github.com/en/actions/quickstart)

---

Thanks to the above step you set up the whole flow for IaC development and easily continue the development. You can quickly run the deployment from VS Code and all changes will be automatically deployed after pushing the repository to GitHub.

In real life, it is a proper time and setup for the development of the required setup. In this workshop, we will deep dive into Bicep details.

---
