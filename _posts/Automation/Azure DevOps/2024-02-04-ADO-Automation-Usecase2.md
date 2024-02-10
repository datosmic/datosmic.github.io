---
title: Work Items Creation Automation Use case 2
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

But manually creating user stories and tasks every month and week respectively is a tediouss task and it takws lot of time. So, Patti decided to automate this work and assigned this work to `Ravi Kiran` 😊. Now Ravi uses his PowerShell expertise to create monthly user stories for each director and weekly tasks to their team members.

`Ravi` has successfully created User Stories for each director. See [Automation Use case for creation of Work Items in Azure DevOps][Automation Use case for creation of Work Items in Azure DevOps] for detailed explanation. Now he wants to create weekly tasks for each team member under each organization director.

## Automation

### Introduction

Create a function `New-WeeklyADOWorkItems` to reuse and for modular coding.

```powershell
function New-WeeklyADOWorkItems {
    param(

    )
}
```

### Alias

Alias can be created for a cmdlet or for a parameter which can be used as a aliasing of that particular entity. Here instead of writing/executing `New-WeeklyADOWorkItems` every time when we want to execute, we can just type `nwado` and provide parameters. To get the list of aliases present in the system can be retrieved by running `Get-Alias` or `gal` (Alias for `Get-Alias`) in terminal. See more about [Get-Alias][Get-Alias].

```powershell
[Alias('nwado')]
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
        ParameterSetName = 'Azure Parameter Set')]
    [ValidateNotNull()]
    [ValidateNotNullOrEmpty()]
    [Alias('type', 'workitem', 'task', 'bug')] 
    $WorkItemType = 'Task',

    # Provide the organization
    [Parameter(Mandatory = $false,
        ParameterSetName = 'Azure Parameter Set')]
    $Organization = "https://dev.azure.com/datosmic/",

    # Provide the organization
    [Parameter(Mandatory = $false,
        ParameterSetName = 'Azure Parameter Set')] 
    $Project =  "datosmic-project1",
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
$OutputLogFilePath = "$outputLogFolderPath\WorkItemCreationLogs_$( (Get-Date).ToString('MMMMdd') ).txt"

Write-Output "`n ================================== Output Logs of $(Get-Date) Iteration ================================== `n" | Out-File -FilePath $OutputLogFilePath -Append

Write-Output "Output Log Path: $($OutputLogFilePath) `n" | Out-File -FilePath $OutputLogFilePath -Append                                            
```

## Determine Active User Stories

To find the respective [User Stories based on the template created before][Automation Use case for creation of Work Items in Azure DevOps] here I am creating an ADO Query. See [Define a work item query in Azure Boards][Define a work item query in Azure Boards] for more information. 

![ADO-WI-Query-Creation][ADO-WI-Query-Creation]

> Only you will be able to query / run this Work Item query/PS script if you have saved this under "My Queries" but if you saved as Shared query, other team members who has access to project will be able to view/run this script.
{: .prompt-tip }

```powershell
# Active User Stories retrieving from the query : https://dev.azure.com/datosmic/datosmic-project1/_queries/query/f8266fd1-fdf3-4b81-ac14-155c4ba116d2/

$ActiveUserStories = az boards query --id 'f8266fd1-fdf3-4b81-ac14-155c4ba116d2' | ConvertFrom-Json # User stories query 

Write-Output "$(Get-Date): Provided values: `nOrganization: $Organization `nProject: $Project `nADO Query used: f8266fd1-fdf3-4b81-ac14-155c4ba116d2" | Out-File -FilePath $OutputLogFilePath -Append
```

> Here I have hard coded the Active User Stories query intentionally as I will not change this evry week/month but if you want to make this as dynamic, you can pass this query id also in the params.
{: .prompt-info }

### Generate Month Name and Generate Title with Starting and Ending Week dates

`Patti` asked `Ravi` to create user stories and tasks in a way that their data analyst team can analyze the efforts/tasks performed by their team and would be able to create visualizations based on the metrics. To acheive this Ravi came up with a plan of templatising the User Stories and Tasks `Title` which can be levereged for data analysis.

User Story Title Template: `[MonthName Year] TeamName Efforts Tracker`  
Example: `[February 2024] Sales & Marketing Efforts Tracker`

```powershell
# Month Name for the retrieving the user story based on title

$FullMonthName = (Get-UICulture).DateTimeFormat.GetMonthName((Get-Date).Month) + ' ' + ((Get-Date).Year)

Write-Output "$(Get-Date): Generated Month Name for  User Story Title: $FullMonthName" | Out-File -FilePath $OutputLogFilePath -Append
```

Work week for Patti's team is from `Monday` to `Sunday` so the tasks title must be created in the similar template with start and end dates.

Task Title Template: `[weekStartDate - weekEndDate] Efforts Tracker - TeamName`  
Example: `[02/05 - 02/11] Efforts Tracker - Sales & Marketing`

```powershell
# Generating Week start date and Week end date for task title in format [ Monday's date in MM/DD - Sunday's date in MM/DD] Efforts Tracker - TeamName
# Determine nearest Monday of the week
$weekStartDate = Get-Date
Write-Output "$(Get-Date): Determining the nearest Monday of the week... `n"
while ($weekStartDate.DayOfWeek -ne 'Monday') { $weekStartDate = $weekStartDate.AddDays(-1) } 
$weekStartDate = $weekStartDate.ToString('MM/dd')

# Determine next Sunday of the week
$weekEndDate = Get-Date
Write-Output "$(Get-Date): Determining the nearest Sunday of the week... `n"
while ($weekEndDate.DayOfWeek -ne 'Sunday') { $weekEndDate = $weekEndDate.AddDays(1) } 
$weekEndDate = $weekEndDate.ToString('MM/dd')

Write-Output "$(Get-Date): Generated Week Start Date and Week End Date for Task Title: `nWeek Start Date: $weekStartDate `nWeek End Date: $weekEndDate" | Out-File -FilePath $OutputLogFilePath -Append
```

> The above code/logic is made in such a way that any day of the week if you run the script it always picks the nearest Monday to coming Sunday.
{: .prompt-info }

### Team Mapping

To create tasks and assign to respective department team member, we need to create a mapping between person, their lead/director and their org. We will use nested Hash tables in PowerShell to create this mapping.

```powershell
$Teams = @{

        Sales            = @{
            TeamName      = 'Sales & Marketing'
            TeamLeadAlias = 'MiriamG'
            TeamMembers   = @('MiriamG', 'MeganB', 'AlexW', 'IsaiahL', 'LynneR', 'AdeleV')
        }
    
        Operations        = @{
            TeamName      = 'Operations'
            TeamLeadAlias = 'NestorW'
            TeamMembers   = @('NestorW', 'JoniS', 'PradeepG', 'DiegoS')
        }
    
        Manufacturing     = @{
            TeamName      = 'Manufacturing'
            TeamLeadAlias = 'LeeG'
            TeamMembers   = @('LeeG', 'HenriettaM', 'GradyA', 'LidiaH', 'JohannaL')
        }

    }
```

### Create Work Item function

See [Automate creation of Work Items in Azure DevOps][Automate creation of Work Items in Azure DevOps] for detailed explanation of creating a function.

### Format title, Call Function and format created Task output 

```powershell
$Tasks = @()
        
$formattedTasks = @()

    foreach ($Team in $Teams.Keys) {

        $TeamMember = $Teams.$Team.TeamMembers
    
        $TeamMember | ForEach-Object {
    
            $UserStoryTitle = "[$FullMonthName] $($Teams.$Team.TeamName) Non-Incidents Efforts Tracker"
            
            $ParentId = ($ActiveUserStories | Where-Object { $_.fields.'System.Title' -eq $UserStoryTitle }).Id
    
            $TaskTitle = "[$weekStartDate - $weekEndDate] Efforts Tracker - $($Teams.$Team.TeamName)" 
            
            Write-Output "$(Get-Date): Passing Values to create Task - [New-ADOWorkItem function]: `nTeamName: $($Teams.$Team.TeamName) `nTeamLeadAlias: $($Teams.$Team.TeamLeadAlias) `nTeamMemberName: $_ `nUserStoryTitle: $UserStoryTitle `nParentId: $ParentId `nTaskTitle: $TaskTitle `nTags: $($Teams.$Team.TeamName) `n" | Out-File -FilePath $OutputLogFilePath -Append
            
            $Tasks += New-ADOWorkItem -Title $TaskTitle -WorkItemType $WorkItemType -AssignedTo $_ -ParentID $ParentId -Tags $($Teams.$Team.TeamName)
    
        }
    }

    # Format tasks to send automated mail 

    $Tasks | ForEach-Object {

        $f = $($_.fields)
        $formattedTask = @(
            [PSCustomObject]@{
                'Work Item ID' = "<a href='https://dev.azure.com/datosmic/datosmic-project1/_workitems/edit/$($_.Id)'>$($_.Id)</a>"
                'Title'        = $($f.'System.Title');
                'Assigned To'  = $($f.'System.AssignedTo'.'displayName');
                'Team'         = $($f.'System.Title'.Split('-')[-1].trim());
                'Project'      = $($f.'System.TeamProject');
                'Iteration'    = $($f.'System.IterationPath');
                'State'        = $($f.'System.State');
                'Type'         = $($f.'System.WorkItemType');
                'Tags'         = $($f.'System.Tags');
                'Parent ID'    = "<a href='https://dev.azure.com/datosmic/datosmic-project1/_workitems/edit/$($f.'System.Parent')'>$($f.'System.Parent')</a>"
                            
            }
        )
    
        $formattedTasks += $formattedTask
    }
```

## Send created new tasks as a mail

Please see [Sending Email using Outlook Desktop Application][Sending Email using Outlook Desktop Application] for more details to send output returned into mail.

![Mail-Output][Mail-Output]

Complete code for the automation can be found at [New-WeeklyADOWorkItems.ps1][New-WeeklyADOWorkItems.ps1] in my GitHub repository.

### See Also 
- [Onboarding to Azure DevOps][Onboarding to Azure DevOps]
- [Getting Started with Azure DevOps using Az CLI][Getting Started with Azure DevOps using Az CLI]
- [Create Work Items in Azure DevOps project using Az CLI][Create Work Items in Azure DevOps project using Az CLI]
- [Update Work Items in Azure DevOps project using Az CLI][Update Work Items in Azure DevOps project using Az CLI]
- [Add relation to Work Items in Azure DevOps project using Az CLI][Add relation to Work Items in Azure DevOps project using Az CLI]
- [Automate creation of Work Items in Azure DevOps][Automate creation of Work Items in Azure DevOps]
- [Automation Use case for creation of Work Items in Azure DevOps][Automation Use case for creation of Work Items in Azure DevOps]
- [Sending Email using Outlook Desktop Application][Sending Email using Outlook Desktop Application]

<!-- Reference Images -->
[Miriam-Graham-Org]: /assets/img/2024-02-04-ADO-Automation-UseCase1/Miriam-Graham-Org.png
[Nestor-Wilke-Org]: /assets/img/2024-02-04-ADO-Automation-UseCase1/Nestor-Wilke-Org.png
[Lee-Gu-Org]: /assets/img/2024-02-04-ADO-Automation-UseCase1/Lee-Gu-Org.png
[CreatedWorkItems]: /assets/img/2024-02-04-ADO-Automation-UseCase1/CreatedWorkItems.png
[ADO-WI-Query-Creation]: /assets/img/2024-02-04-ADO-Automation-Usecase2/ADO-WI-Query-Creation.png
[Mail-Output]: /assets/img/2024-02-04-ADO-Automation-Usecase2/Mail-Output.png

<!-- Reference Links -->
[Automation Use case for creation of Work Items in Azure DevOps]: /posts/ADO-Automation-Usecase1/
[Define a work item query in Azure Boards]: https://learn.microsoft.com/en-us/azure/devops/boards/queries/using-queries?view=azure-devops
[Automate creation of Work Items in Azure DevOps]: /posts/Create-WorkItems-Automation/
[Sending Email using Outlook Desktop Application]: /posts/Send-Email-Automation-PS/
[Onboarding to Azure DevOps]: /posts/Azure-DevOps-Onboard/
[Getting Started with Azure DevOps using Az CLI]: /posts/Azure-DevOps-CLI/
[Create Work Items in Azure DevOps project using Az CLI]: /posts/Create-ADO-WorkItems-CLI/
[Update Work Items in Azure DevOps project using Az CLI]: /posts/Update-ADO-WorkItems-CLI/
[Add relation to Work Items in Azure DevOps project using Az CLI]: /posts/Add-Relation-ADO-WorkItems-CLI/
[Automate creation of Work Items in Azure DevOps]: /posts/Create-WorkItems-Automation/
[New-WeeklyADOWorkItems.ps1]: https://github.com/scvslsravikiran/Blogs/blob/b2c6158e35e744a5bf517d94291fadc9eb102ba9/New-WeeklyADOWorkItems.ps1