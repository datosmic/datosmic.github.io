---
title: Sending Email using Outlook Desktop Application
date: 2024-02-05 15:30 +0530
categories: [Automation, PowerShell]
tags: [Automation, PowerShell]
---

## Introduction

Sending an email from Outlook Desktop client (Outlook Application) is easy. Here we use .NET libraries to open and send the mail using various operations. You can send this mail in plain text or HTML content as we generally do from outlook.

## Table styling for mail
```powershell
# Table styling for mail
$styling = @"
<style>
th, td {
    padding: 12px;
    }
    table, th, td {
    border: 1px solid black;
    border-collapse: collapse;
    text-align:center;
}
th {
    background-color: #008080;
    color:white;
}
li {
    color:#470FF4;
}
</style>
"@
```
       
## Mail Content

Mail content is the place where we can keep the HTML content with Intro statements (Headers), Notes (footers) and main content.

```powershell
$IntroStatement = @"
<p style='margin:0in;font-size:15px;font-family:"Calibri",sans-serif;color:#470FF4;'>Hello Team,</p>
<br/>
<ol style="margin-bottom:0in;margin-top:0in;" start="1" type="1">
    <li style='margin-top:0in;margin-right:0in;margin-bottom:0in;margin-left:0in;font-size:15px;font-family:"Calibri",sans-serif;'>Please find  <strong>MS EDGE</strong> instances running in the machine currently.</li>
</ol>
<br/>
"@

$Note = "<br/><br/><p style='color:#470FF4;'><i>Note: This is an auto generated Email using PowerShell sent by Ravi Kiran Srikantam. Please reach out to <a href='mailto:scvslsravikiran@tm8h1.onmicrosoft.com'>Ravi Kiran Srikantam</a> for any queries/clarifications. Have a great day!</i></p>"

$Content = $process | ConvertTo-Html -As Table -PreContent "$IntroStatement" -Head $styling -PostContent $Note

$HTMLMailContent = $Content -replace ('&lt;','<') -replace('&gt;','>') -replace ('&#39;','') 
```

## Create an instance of Outlook.Application COM object

Creating a COM object for Outlook application instance.

```powershell
$outlook = New-Object -ComObject Outlook.Application

# Get the MAPI namespace
$namespace = $outlook.GetNamespace('MAPI')
```

## Construct email item object

Email object to send mails. Here we can configure Subject, Body format, To, Cc, Bcc, sensitivity etc.

```powershell
$mailItem = $outlook.CreateItem(0)
$mailItem.Subject = "[DatOsmic Team] Processes of MS Edge - $(Get-Date)"

$mailItem.BodyFormat = 2 # HTML Format
$mailItem.HTMLBody = "$($HTMLMailContent)"
$mailItem.To = ""
$mailItem.Cc = ""
$mailItem.Sensitivity = 2

$mailItem.Display()
$mailItem.Save()
# $mailItem.Send() # Uncomment to directly send mail
```

> Use `$mailItem.BodyFormat = 1` for plain text format and `$mailItem.BodyFormat = 2` for HTML Format.
{: .prompt-tip }


## Release the COM objects

Releasing objects after completion of sending mail. This is like closing connections after our work.

```powershell
[System.Runtime.Interopservices.Marshal]::ReleaseComObject($mailItem) | Out-Null
[System.Runtime.Interopservices.Marshal]::ReleaseComObject($namespace) | Out-Null
[System.Runtime.Interopservices.Marshal]::ReleaseComObject($outlook) | Out-Null
```


