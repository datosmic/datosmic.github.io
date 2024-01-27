---
title: Update Work Items in Azure DevOps project using Az CLI
date: 2024-01-27 18:30 +0530
categories: [DevOps, Azure DevOps]
tags: [Azure DevOps, Azure, DevOps, Azure CLI, PowerShell]
---

If we want to assign the un-assigned work items to a specific team member who is part of ADO Organization or to re-assign an existing work item to some other team member due to priority fixes etc.. we can use `az boards work-item update` and make sure to provide `--id` parameter which is mandatory.

## Update Work Item

The command to run is `az boards work-item update` to update work items. The complete syntax of command is shown below:

```bash
az boards work-item update --id
                           [--area]
                           [--assigned-to]
                           [--description]
                           [--detect {false, true}]
                           [--discussion]
                           [--fields]
                           [--iteration]
                           [--open]
                           [--org]
                           [--reason]
                           [--state]
                           [--title]
```

With this update command, we can update the following fields directly:
1. Assigned To (Owner)
2. Area Path
3. Description (Summary)
4. Discussion (Comments)
5. Iteration
6. State
7. Title
8. Fields

## Example commands to update work items

### Assigned To (Owner)

Owner/Assigned To field can be directly updated using `--assigned-to` parameter. Here is the sample code:
```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az boards work-item update --id 2 `
    --assigned-to 'AdeleV' `
    --org $organization 
```

> You might receive this error while trying to run the above command: `The identity value '<<UserAlias>>' for field 'Assigned To' is an unknown identity.`. This means the user alias you are trying to assign is not in the scope of this organization (Azure DevOps). You might also see this similar error from UI (Azure DevOps portal) as shown below:
> ![ADO-WI-Assign-Error][ADO-WI-Assign-Error]
> To fix this issue, please check the permissions of the user you are trying to add like if the user exists in the organization or user alias trying to add is incorrect etc. 
{: .prompt-info }

> If the user alias is not added in the Azure DevOps organization/project, then please add alias in respective group to provide permissions (contributors/Build Admins etc..). See [Security groups, service accounts, and permissions in Azure DevOps][Security groups, service accounts, and permissions in Azure DevOps], [Add users or groups to a team or project][Add users or groups to a team or project] and [View permissions for yourself or others][View permissions for yourself or others] for more information.
{: .prompt-tip }

### Area Path

Area Path (Area) field can be directly updated using `--area` parameter. Here is the sample code:
```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az boards work-item update --id 2 `
    --org $organization `
    --area 'datosmic-project1\datosmic' 
```

### Iteration

Iteration field can be directly updated using `--iteration` parameter. Here is the sample code:
```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az boards work-item update --id 2 `
    --org $organization `
    --iteration 'datosmic-project1\Iteration 1'
```

### Description

Description field can be directly updated using `--description` parameter. This accepts HTML content so we can pass the complete data in HTML format and update it. Here is the sample code:
```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az boards work-item update --id 2 `
    --org $organization `
    --description "<h1> This is Description with H1 Heading </h1>"
```

### Discussion

Description field can be directly updated using `--discussion` parameter. This accepts HTML content so we can pass the complete data in HTML format and update it. Here is the sample code:
```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az boards work-item update --id 2 `
    --org $organization `
    --discussion "<h3> This is a comment in discussion section </h3>"
```

### State

State field can be directly updated using `--state` parameter. This accepts only allowed values. Here is the sample code:
```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az boards work-item update --id 2 `
    --org $organization `
    --state "Active"
```

### Fields

Various field values of a work item can be updated using this value and Space separated `"field=value"` pairs for custom fields we would like to set. Refer [https://aka.ms/azure-devops-cli-field-api](https://aka.ms/azure-devops-cli-field-api) for more details on fields.

Syntax:
```http
GET https://dev.azure.com/{organization}/{project}/_apis/wit/fields?api-version=5.0
```
Example: `https://dev.azure.com/datosmic/datosmic-project1/_apis/wit/fields?api-version=5.0`


> Its not mandatory that these need to be executed individually, these can be ran in single command by providing valid parameters to update everything at single execution/run.
{: .prompt-tip }



<!-- Reference Images -->
[ADO-WI-Assign-Error]: /assets/img/2024-01-27-Update-ADO-WorkItems-CLI/ADO-WI-Assign-Error.png


<!-- Reference Links -->
[Security groups, service accounts, and permissions in Azure DevOps]: https://learn.microsoft.com/en-us/azure/devops/organizations/security/permissions?view=azure-devops
[Add users or groups to a team or project]: https://learn.microsoft.com/en-us/azure/devops/organizations/security/add-users-team-project?view=azure-devops
[View permissions for yourself or others]: https://learn.microsoft.com/en-us/azure/devops/organizations/security/view-permissions?view=azure-devops
