---
title: Automation Use case for creation of Work Items in Azure DevOps
date: 2024-02-04 16:30 +0530
categories: [Automation, Azure DevOps]
tags: [Automation, Azure DevOps, DevOps, Azure CLI, PowerShell, Project/Usecase]
---

## Use-case / Scenario

`Patti Fernandez` is a President of **DatOsmic** organization and she wants to track the monthly/weekly wise tasks/efforts of her team members. For this, she wants to create a monthly User Story for each director under her and also wants to create a task to each team member under each directors' organization for every week.

**Miriam Graham Organiation**  

![Miriam-Graham-Org][Miriam-Graham-Org]  

**Nestor Wilke Organization**  

![Nestor-Wilke-Org][Nestor-Wilke-Org]

**Lee Gu Organization**

![Lee-Gu-Org][Lee-Gu-Org]

But manually creating user stories and tasks every month and week respectively is a tediouss task and it takws lot of time. So, Patti decided to automate this work and assigned this work to `Ravi Kiran` 😊. Now Ravi uses his PowerShell expertise to create monthly user stories for each director.

## Automation

### Introduction

Create a function `New-MonthlyADOUserStories` to reuse and for modular coding.

```powershell
function New-MonthlyADOUserStories {
    param(

    )
}
```

### Alias

Alias can be created for a cmdlet or for a parameter which can be used as a aliasing of that particular entity. Here instead of writing/executing `New-MonthlyADOUserStories` every time when we want to execute, we can just type `nwus` and provide parameters. To get the list of aliases present in the system can be retrieved by running `Get-Alias` or `gal` (Alias for `Get-Alias`) in terminal. See more about [Get-Alias][Get-Alias].

```powershell
[Alias('nmus')]
```

### Parameters

Function contains various parameters, here I am keeping all parametes as mandatory `[Parameter(Mandatory = $false` but you can change it to true `[Parameter(Mandatory = $true` or leave `Mandatory` parameter which defaults to false and update the rest of code accordingly to your usecase scenario.

```powershell
param (
# Provide the work item type
[Parameter(Mandatory = $false, 
    ValueFromPipeline = $true,
    ValueFromPipelineByPropertyName = $true, 
    ValueFromRemainingArguments = $false, 
    ParameterSetName = 'ADO Work Item Creation')]
[ValidateNotNull()]
[ValidateNotNullOrEmpty()]
[Alias('type', 'workitem', 'task', 'bug')] 
$WorkItemType = 'User Story',

[Parameter(Mandatory = $false,
    ParameterSetName = 'ADO Work Item Creation')] 
$organization,

[Parameter(Mandatory = $false,
    ParameterSetName = 'ADO Work Item Creation')] 
$Project
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
```

### Title of the User Story

`Patti` asked `Ravi` to create user stories and tasks in a way that their data analyst team can analyze the efforts/tasks performed by their team and would be able to create visualizations based on the metrics. To acheive this Ravi came up with a plan of templatising the User Stories and Tasks `Title` which can be levereged for data analysis.

Title Template: `[MonthName Year] TeamName Efforts Tracker`  
Example: `[February 2024] Sales & Marketing Efforts Tracker`

To generate this template format, lets find out the month and year:  

```powershell
Write-Output "$(Get-Date): Determining Month Name" | Out-File -FilePath $OutputLogFilePath -Append
        
$FullMonthName = (Get-UICulture).DateTimeFormat.GetMonthName((Get-Date).Month) + ' ' + ((Get-Date).Year)

Write-Output "$(Get-Date): Determined Month name: $FullMonthName" | Out-File -FilePath $OutputLogFilePath -Append
```

### Team Mapping

To create user stories and assign to respective department director/lead, we need to create a mapping between person and his org. We will use Hash tables in PowerShell to create this mapping.

```powershell
  $TeamMapping = @{
            Sales         = @('Sales & Marketing', 'miriamg')
            Operations    = @('Operations', 'nestorw')
            Manufacturing = @('Manufacturing', 'leeg')
        }
```

### Create Work Item function

See [Automate creation of Work Items in Azure DevOps][Automate creation of Work Items in Azure DevOps] for detailed explanation of creating a function.

### Title format and Calling function to create work item and update necessary details & relationships

```powershell
$WorkItemDetails = @()
foreach ($TeamName in $TeamMapping.Keys) {
    
    $OrgName = $TeamMapping.$TeamName[0]

    $OrgLeadAlias = $TeamMapping.$TeamName[1]

    $Title = "[$FullMonthName] $OrgName Efforts Tracker"
    
    Write-Output "$(Get-Date): Generated Values: `nFullMonthName: $FullMonthName `nOrgName:$($TeamMapping.$TeamName[0]) `nTitle: $Title `nOrgLeadAlias: $($TeamMapping.$TeamName[1]) `n" | Out-File -FilePath $OutputLogFilePath -Append

    $WorkItemDetails += New-ADOWorkItem -Title $Title -WorkItemType $WorkItemType -AssignedTo $OrgLeadAlias -ParentID $ParentId -Tags $OrgName -organization $organization -Project $Project
}
```

![CreatedWorkItems][CreatedWorkItems]

Complete code for the automation can be found at [New-MonthlyADOUserStories.ps1][New-MonthlyADOUserStories.ps1] in my GitHub repository.

### See Also 
- [Onboarding to Azure DevOps][Onboarding to Azure DevOps]
- [Getting Started with Azure DevOps using Az CLI][Getting Started with Azure DevOps using Az CLI]
- [Create Work Items in Azure DevOps project using Az CLI][Create Work Items in Azure DevOps project using Az CLI]
- [Update Work Items in Azure DevOps project using Az CLI][Update Work Items in Azure DevOps project using Az CLI]
- [Add relation to Work Items in Azure DevOps project using Az CLI][Add relation to Work Items in Azure DevOps project using Az CLI]
- [Automate creation of Work Items in Azure DevOps][Automate creation of Work Items in Azure DevOps]

<!-- Reference Images -->
[Miriam-Graham-Org]: /assets/img/2024-02-04-ADO-Automation-UseCase1/Miriam-Graham-Org.png
[Nestor-Wilke-Org]: /assets/img/2024-02-04-ADO-Automation-UseCase1/Nestor-Wilke-Org.png
[Lee-Gu-Org]: /assets/img/2024-02-04-ADO-Automation-UseCase1/Lee-Gu-Org.png
[CreatedWorkItems]: /assets/img/2024-02-04-ADO-Automation-UseCase1/CreatedWorkItems.png

<!-- Reference Links -->
[Get-Alias]: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-alias?view=powershell-7.4
[Automate creation of Work Items in Azure DevOps]: /posts/Create-WorkItems-Automation/
[New-MonthlyADOUserStories.ps1]: https://github.com/scvslsravikiran/Blogs/blob/295c053e3a779ed1b9068d70f57b1923f0712e73/New-MonthlyADOUserStories.ps1
[Onboarding to Azure DevOps]: /posts/Azure-DevOps-Onboard/
[Getting Started with Azure DevOps using Az CLI]: /posts/Azure-DevOps-CLI/
[Create Work Items in Azure DevOps project using Az CLI]: /posts/Create-ADO-WorkItems-CLI/
[Update Work Items in Azure DevOps project using Az CLI]: /posts/Update-ADO-WorkItems-CLI/
[Add relation to Work Items in Azure DevOps project using Az CLI]: /posts/Add-Relation-ADO-WorkItems-CLI/
[Automate creation of Work Items in Azure DevOps]: /posts/Create-WorkItems-Automation/