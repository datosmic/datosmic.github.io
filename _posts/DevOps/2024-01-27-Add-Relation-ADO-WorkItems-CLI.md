---
title: Add relation to Work Items in Azure DevOps project using Az CLI
date: 2024-01-27 21:30 +0530
categories: [DevOps, Azure DevOps]
tags: [Azure DevOps, Azure, DevOps, Azure CLI, PowerShell]
---

There are multiple relationships for a work item. The complete list of relations of work item can be retrieved by running `az boards work-item relation list-type` command. 

```bash
az boards work-item relation list-type [--detect {false, true}]
                                       [--org]
```

## Create a Work Item

Here I am directly creating a work item with the parameters and storing in a variable. For detailed explanation to create work item can be found in [Create Work Items in Azure DevOps project using Az CLI][Create Work Items in Azure DevOps project using Az CLI].

```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

$NewWorkItem = az boards work-item create `
    --org $organization `
    --project $project `
    --title "User Story 1" `
    --type "User Story" `
    --assigned-to "scvslsravikiran" `
    --description "<h3> This is sample description for the User Story </h3>" `
    --discussion "<h3> Created this User Story with Automation using Az CLI</h3>" `
    --fields System.Tags="TestTag" `
    --iteration 'datosmic-project1\Iteration 2' `
    --open
```

The output present in `$NewWorkItem` is in JSON format and we can convert this to an object to access elements easily present in an object.

```powershell
$WorkItem = ($NewWorkItem | ConvertFrom-Json)
```

Retrieve the WorkItem Id from the above ouput object stored in `$WorkItem`.

```powershell
$WorkItemId = $WorkItem.Id
```

## Adding Relation to Work Item

Adding a relation to a work item can be done using `az boards work-item relation add` command. Here is the syntax of the command:

```bash
az boards work-item relation add --id
                                 --relation-type
                                 [--detect {false, true}]
                                 [--org]
                                 [--target-id]
                                 [--target-url]
```

Now we will add the User Story `User Story 1` with `$WorkItemId` as 4 to Existing `Epic` with Id as 1 and stored in `$WorkItemDetails` variable.

```powershell
$WorkItemDetails = az boards work-item relation add `
                --org $organization `
                --id $WorkItemId `
                --relation-type parent `
                --target-id $ParentId 
$WorkItemDetails
```

Here is the complete output from Azure DevOps portal:

![Add-Relation-ADO-WorkItems-CLI][Add-Relation-ADO-WorkItems-CLI]

The complete script looks like below:

```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

$NewWorkItem = az boards work-item create `
            --org $organization `
            --project $project `
            --title "User Story 1" `
            --type "User Story" `
            --assigned-to "scvslsravikiran" `
            --description "<h3> This is sample description for the User Story </h3>" `
            --discussion "<h3> Created this User Story with Automation using Az CLI</h3>" `
            --fields System.Tags="TestTag" `
            --iteration 'datosmic-project1\Iteration 2' `
            --openA

$WorkItem = ($NewWorkItem | ConvertFrom-Json)
$WorkItemId = $WorkItem.Id

$ParentId = 1

$WorkItemDetails = az boards work-item relation add `
                --org $organization `
                --id $WorkItemId `
                --relation-type parent `
                --target-id $ParentId 
$WorkItemDetails
```

<!-- Reference Images -->
[Add-Relation-ADO-WorkItems-CLI]: /assets/img/2024-01-27-Add-Relation-ADO-WorkItems-CLI/Add-Relation-ADO-WorkItems-CLI.png

<!-- Reference Links -->
[Create Work Items in Azure DevOps project using Az CLI]: /posts/Create-ADO-WorkItems-CLI/

