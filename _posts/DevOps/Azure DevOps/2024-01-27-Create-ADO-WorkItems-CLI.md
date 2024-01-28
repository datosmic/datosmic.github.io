---
title: Create Work Items in Azure DevOps project using Az CLI
date: 2024-01-27 14:30 +0530
categories: [DevOps, Azure DevOps]
tags: [Azure DevOps, Azure, DevOps, Azure CLI, PowerShell]
---

To create work items in Azure DevOps we need to have an Azure DevOps organization and project. If you don't have either of these, please follow steps in [Onboarding to Azure DevOps][Onboarding to Azure DevOps] and [Getting Started with Azure DevOps using Az CLI][Getting Started with Azure DevOps using Az CLI] to onboard Azure DevOps and install Azure DevOps (ADO) extension to Az CLI.

### Create work item 

The command to run is `az boards work-item create` to create work items. The complete syntax of command is shown below:

```bash
az boards work-item create --title
                           --type
                           [--area]
                           [--assigned-to]
                           [--description]
                           [--detect {false, true}]
                           [--discussion]
                           [--fields]
                           [--iteration]
                           [--open]
                           [--org]
                           [--project]
                           [--reason]
```

Here is the complete command to run using PowerShell to create a work item. I am trying to create an `Epic` work item type here:

```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az boards work-item create `
    --title "Epic1" `
    --type "Epic" `
    --org $organization `
    --project $project
```

> If you want to create a `Task` type, you can replace `--type "Epic"` with `--type "Task"` in the above command.
{: .prompt-tip }

> The backtick "**`**" used in the command is just to let shell know the continuation of the command in different/multiple lines for better readibility purpose. The above same command can be also written/executed as below:  
> ```powershell
> $organization = "https://dev.azure.com/datosmic/"
> $project = "datosmic-project1"
>
> az boards work-item create --title "Epic1" --type "Epic" --org $organization --project $project`
> ```
{: .prompt-info }

Once the command has successfully ran, we would see an JSON output of complete work item details. We can see the Work Item `CreatedDate`, `ID`, `State`, `Type`, `Title`, `Project` etc.. from the JSON output.

![ADO-WI-Creation][ADO-WI-Creation]

We can navigate to Azure DevOps portal to see the work item created.

![ADO-WI][ADO-WI]

![ADO-WI2][ADO-WI2]

### See Also 
- [Onboarding to Azure DevOps][Onboarding to Azure DevOps]
- [Getting Started with Azure DevOps using Az CLI][Getting Started with Azure DevOps using Az CLI]
- [Update Work Items in Azure DevOps project using Az CLI][Update Work Items in Azure DevOps project using Az CLI]
- [Add relation to Work Items in Azure DevOps project using Az CLI][Add relation to Work Items in Azure DevOps project using Az CLI]
- [Automate creation of Work Items in Azure DevOps][Automate creation of Work Items in Azure DevOps]


<!-- Reference Images -->
[ADO-WI-Creation]: /assets/img/2024-01-27-Create-ADO-WorkItems-CLI/ADO-WI-Creation.png
[ADO-WI]: /assets/img/2024-01-27-Create-ADO-WorkItems-CLI/ADO-WI.png
[ADO-WI2]: /assets/img/2024-01-27-Create-ADO-WorkItems-CLI/ADO-WI2.png

<!-- Reference Links -->
[Onboarding to Azure DevOps]: /posts/Azure-DevOps-Onboard/
[Getting Started with Azure DevOps using Az CLI]: /posts/Azure-DevOps-CLI/
[Update Work Items in Azure DevOps project using Az CLI]: /posts/Update-ADO-WorkItems-CLI/
[Add relation to Work Items in Azure DevOps project using Az CLI]: /posts/Add-Relation-ADO-WorkItems-CLI/
[Automate creation of Work Items in Azure DevOps]: /posts/Create-WorkItems-Automation/
[Getting Started with Azure DevOps using Az CLI]: /posts/Azure-DevOps-CLI/