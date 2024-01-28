---
title: Automate creation of Work Items in Azure DevOps 
date: 2024-01-28 06:30 +0530
categories: [Automation, Azure DevOps]
tags: [Automation, Azure DevOps, Azure, DevOps, Azure CLI, PowerShell]
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

## Automation

### Introduction

Create a function `New-ADOWorkItem` to reuse and for modular coding.

```powershell
function New-ADOWorkItem {
    param(

    )
}
```

### Parameters

Function contains various parameters, here I am keeping all parametes as mandatory `[Parameter(Mandatory = $true` but you can change it to false `[Parameter(Mandatory = $false` or leave `Mandatory` parameter which defaults to false and update the rest of code accordingly to your usecase scenario.
```powershell
param (
    [Parameter(Mandatory = $true,
        ParameterSetName = 'ADO Work Item Creation')] 
    $Title,
    
    [Parameter(Mandatory = $true,
        ParameterSetName = 'ADO Work Item Creation')] 
    $WorkItemType,
    
    [Parameter(Mandatory = $true,
        ParameterSetName = 'ADO Work Item Creation')] 
    $AssignedTo,
    
    [Parameter(Mandatory = $true,
        ParameterSetName = 'ADO Work Item Creation')] 
    $ParentID,
    [Parameter(Mandatory = $true,
        ParameterSetName = 'ADO Work Item Creation')] 
    $Tags
)
```

### Configure defaults (Organization/Project)

We can configure default Organization and Project and use `--detect true` parameter in commands or declare them as values and provide respective required values in various commands:
```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"
```
or 

```powershell
$organization = "https://dev.azure.com/datosmic/"
$project = "datosmic-project1"

az devops configure --defaults organization=$organization project=$project
```

### Logging

Logging is very importatnt while scripting/automation so that if we want to debug some errors or some execution details we would be able to identify the exact issue.

Here I am testing for a path if exists then create loggnig file else create a folder for logging in OneDrive. `Get-Date` function is used to provide the exact time stamp of the log executed for our reference.

> You can update the location of logs to store and syntax of logs as per your requirement.
{: .prompt-tip }

```powershell
$outputLogFolderPath = "$env:OneDrive\Automation"

if ( (Test-Path -PathType container $outputLogFolderPath) -eq $false ) {
    Write-Host "$(Get-Date): $($outputLogFolderPath) doesn't exists so continuing with creation of folder for log file"
    New-Item -ItemType Directory -Path $outputLogFolderPath
}
else {
    Write-Host "$(Get-Date): $($outputLogFolderPath) exists so continuing with creation of log file"
}

# Output log file for generating output into file
$OutputLogFilePath = "$outputLogFolderPath\TasksCreationLogs_$( (Get-Date).ToString('MMMMdd') ).txt"

Write-Output "$(Get-Date): *** Creating Work Item ***`n" | Out-File -FilePath $OutputLogFilePath -Append
```

### Create

```powershell
$WorkItem = az boards work-item create `
            --org $organization `
            --project $project `
            --title $Title `
            --type $WorkItemType `
            --assigned-to $AssignedTo 

Write-Verbose "$(Get-Date): Created Work Item: `n$WorkItem"
```

### Add Relation, Tags and output the Task details

Here we are checking if the above work item status is success or not then converting the output from JSON as every command outputs in JSON format then add tags and formatting output with required details.

```powershell
# Adding Relation to the Work Item
if ($? -eq $true) {
    
    $WorkItemID = ($WorkItem | ConvertFrom-Json).Id

    Write-Output "$(Get-Date): Passed values in New-ADOWorkItem function: `nWorkItemID: $($WorkItemID) `nTitle: $($Title) `nWorkItemType: $($WorkItemType) `nAssignedTo: $($AssignedTo) `nParentID: $ParentID `nTags: $($Tags) `n" | Out-File -FilePath $OutputLogFilePath -Append
    
    $WorkItemDetails = az boards work-item relation add `
        --org $organization `
        --id $WorkItemID  `
        --relation-type parent `
        --target-id $ParentID  

    if ($? -eq $true) {   
        Write-Output "$(Get-Date): Added $ParentID User Story as a Parent for $WorkItemID Task... `nWorkItemDetails: $WorkItemDetails `n " | Out-File -FilePath $OutputLogFilePath -Append
        
        $Tags = $Tags

        $Task = az boards work-item update `
            --org $organization `
            --fields System.Tags="$Tags" `
            --id $WorkItemID  

        if ($? -eq $true) { 
            Write-Output "$(Get-Date): Added Tags: $($Task.fields.'System.Tags') to the Task $($Task.Id)..`nTask Details after adding Tags: $Task `n" | Out-File -FilePath $OutputLogFilePath -Append

            $Task = $Task | ConvertFrom-Json

            Write-Host "$(Get-Date): Successfully created `
            Title: $($Task.fields.'System.Title') `
            WorkItem Id: $($Task.Id) `
            WorkItem URL: $($Task.url) `
            ParentID: $ParentID `
            Relation Type: $($Task.relations.attributes.name) `
            Related WorkItem URL: $($Task.relations.url) `
            WorkItem Type: $($Task.fields.'System.WorkItemType') `
            IterationPath:  $($Task.fields.'System.IterationPath') `
            State: $($Task.fields.'System.State') `
            Tags: $($Task.fields.'System.Tags') `
            AssignedTo: $($Task.fields.'System.AssignedTo'.displayName) `n`n" -ForegroundColor Cyan
        
            Write-Output "$(Get-Date): Successfully created `
            Title: $($Task.fields.'System.Title') `
            WorkItem Id: $($Task.Id) `
            WorkItem URL: $($Task.url) `
            ParentID: $ParentID `
            Relation Type: $($Task.relations.attributes.name) `
            Related WorkItem URL: $($Task.relations.url) `
            WorkItem Type: $($Task.fields.'System.WorkItemType') `
            IterationPath:  $($Task.fields.'System.IterationPath') `
            State: $($Task.fields.'System.State') `
            Tags: $($Task.fields.'System.Tags') `
            AssignedTo: $($Task.fields.'System.AssignedTo'.displayName) `n`n" | Out-File -FilePath $OutputLogFilePath -Append

            return $Task
        }
        else {
            Write-Error "$(Get-Date): Work Item creation failed with error at Task."
        }
    }
    else {
        Write-Error "$(Get-Date): Work Item creation failed with error at WorkItemDetails"
    }
}
else {
    Write-Error "$(Get-Date): Work Item creation failed with error at WorkItem"
}
```

### Calling function to create work item and update necessary details & relationships

```powershell
$Title = "Created Work Item with Automation"
$WorkItemType = "Task"
$AssignedTo = "AdeleV"
$ParentId = 3
$Tags = "Automation;Task"

$Task = New-ADOWorkItem -Title $Title -WorkItemType $WorkItemType -AssignedTo $AssignedTo -ParentID $ParentId -Tags $Tags
```

Complete code for the automation can be found at [New-ADOWorkItem.ps1][New-ADOWorkItem.ps1] in my GitHub repository.

### See Also 
- [Onboarding to Azure DevOps][Onboarding to Azure DevOps]
- [Getting Started with Azure DevOps using Az CLI][Getting Started with Azure DevOps using Az CLI]
- [Create Work Items in Azure DevOps project using Az CLI][Create Work Items in Azure DevOps project using Az CLI]
- [Update Work Items in Azure DevOps project using Az CLI][Update Work Items in Azure DevOps project using Az CLI]
- [Add relation to Work Items in Azure DevOps project using Az CLI][Add relation to Work Items in Azure DevOps project using Az CLI]


<!-- Reference Links -->
[Onboarding to Azure DevOps]: /posts/Azure-DevOps-Onboard/
[Getting Started with Azure DevOps using Az CLI]: /posts/Azure-DevOps-CLI/
[Create Work Items in Azure DevOps project using Az CLI]: /posts/Create-ADO-WorkItems-CLI/
[Update Work Items in Azure DevOps project using Az CLI]: /posts/Update-ADO-WorkItems-CLI/
[Add relation to Work Items in Azure DevOps project using Az CLI]: /posts/Add-Relation-ADO-WorkItems-CLI/
[New-ADOWorkItem.ps1]: https://github.com/scvslsravikiran/Blogs/blob/413ae5ac57e53f64facec9efbf66f7fc5f9770fe/New-ADOWorkItem.ps1