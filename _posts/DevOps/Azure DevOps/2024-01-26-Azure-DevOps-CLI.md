---
title: Getting Started with Azure DevOps using Az CLI
date: 2024-01-26 20:30 +0530
categories: [DevOps, Azure DevOps]
tags: [Azure DevOps, Azure, DevOps, Azure CLI, PowerShell]
---

## Install Azure CLI

The [Azure Command-Line Interface (CLI)][Az CLI] is a cross-platform command-line tool to connect to Azure and execute administrative commands on Azure resources. It allows the execution of commands through a terminal using interactive command-line prompts or a script.

We can [install Az CLI][Install Az CLI] in [Windows][Install Az CLI-Win], [Linux][Install Az CLI-Linux] or [Mac OS][Install Az CLI-Mac].  

Here are the windows installation steps  

![Azure-CLI-Installation][Azure-CLI-Installation]

![Azure-CLI-Installation2][Azure-CLI-Installation2]

## Azure CLI Help

To get familiar with the syntax and commands of Az CLI, we can just type `Az` in the terminal and we get the help. Your output look like below. To learn A-Z about Az CLI, please see [A-Z Az CLI][A-Z Az CLI].

```bash

     /\
    /  \    _____   _ _  ___ _
   / /\ \  |_  / | | | \'__/ _\
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome to the cool new Azure CLI!

Use `az --version` to display the current version.
Here are the base commands:

    account             : Manage Azure subscription information.
    acr                 : Manage private registries with Azure Container Registries.
    ad                  : Manage Azure Active Directory Graph entities needed for Role Based Access
                         Control.
    advisor             : Manage Azure Advisor.
    afd                 : Manage Azure Front Door Standard/Premium.
    aks                 : Manage Azure Kubernetes Services.
    ams                 : Manage Azure Media Services resources.
    apim                : Manage Azure API Management services.
    appconfig           : Manage App Configurations.
    appservice          : Manage App Service plans.
    aro                 : Manage Azure Red Hat OpenShift clusters.
    backup              : Manage Azure Backups.
    batch               : Manage Azure Batch.
    bicep               : Bicep CLI command group.
    billing             : Manage Azure Billing.
    bot                 : Manage Microsoft Azure Bot Service.
    cache               : Commands to manage CLI objects cached using the `--defer` argument.
    capacity            : Manage capacity.
    cdn                 : Manage Azure Content Delivery Networks (CDNs).
    cloud               : Manage registered Azure clouds.
    cognitiveservices   : Manage Azure Cognitive Services accounts.
    config              : Manage Azure CLI configuration.
    configure           : Manage Azure CLI configuration. This command is interactive.
    connection          : Commands to manage Service Connector local connections which allow local
                         environment to connect Azure Resource. If you want to manage connection for
                         compute service, please run 'az webapp/containerapp/spring connection'.
    consumption         : Manage consumption of Azure resources.
    container           : Manage Azure Container Instances.
    containerapp        : Manage Azure Container Apps.
    cosmosdb            : Manage Azure Cosmos DB database accounts.
    databoxedge         : Support data box edge device and management.
    deployment          : Manage Azure Resource Manager template deployment at subscription scope.
    deployment-scripts  : Manage deployment scripts at subscription or resource group scope.
    disk                : Manage Azure Managed Disks.
    disk-access         : Manage disk access resources.
    disk-encryption-set : Disk Encryption Set resource.
    dla                 : Manage Data Lake Analytics accounts, jobs, and catalogs.
    dls                 : Manage Data Lake Store accounts and filesystems.
    dms                 : Manage Azure Data Migration Service (classic) instances.
    eventgrid           : Manage Azure Event Grid topics, domains, domain topics, system topics
                         partner topics, event subscriptions, system topic event subscriptions and
                         partner topic event subscriptions.
    eventhubs           : Eventhubs.
    extension           : Manage and update CLI extensions.
    feature             : Manage resource provider features.
    feedback            : Send feedback to the Azure CLI Team.
    find                : I'm an AI robot, my advice is based on our Azure documentation as well as
                         the usage patterns of Azure CLI and Azure ARM users. Using me improves
                         Azure products and documentation.
    functionapp         : Manage function apps. To install the Azure Functions Core tools see
                         https://github.com/Azure/azure-functions-core-tools.
    group               : Manage resource groups and template deployments.
    hdinsight           : Manage HDInsight resources.
    identity            : Managed Identities.
    image               : Manage custom virtual machine images.
    interactive         : Start interactive mode. Installs the Interactive extension if not
                         installed already.
    iot                 : Manage Internet of Things (IoT) assets.
    keyvault            : Manage KeyVault keys, secrets, and certificates.
    kusto               : Manage Azure Kusto resources.
    lab                 : Manage Azure DevTest Labs.
    lock                : Manage Azure locks.
    logicapp            : Manage logic apps.
    login               : Log in to Azure.
    logout              : Log out to remove access to Azure subscriptions.
    managed-cassandra   : Azure Managed Cassandra.
    managedapp          : Manage template solutions provided and maintained by Independent Software
                         Vendors (ISVs).
    managedservices     : Manage the registration assignments and definitions in Azure.
    maps                : Manage Azure Maps.
    mariadb             : Manage Azure Database for MariaDB servers.
    monitor             : Manage the Azure Monitor Service.
    mysql               : Manage Azure Database for MySQL servers.
    netappfiles         : Manage Azure NetApp Files (ANF) Resources.
    network             : Manage Azure Network resources.
    policy              : Manage resource policies.
    postgres            : Manage Azure Database for PostgreSQL servers.
    ppg                 : Manage Proximity Placement Groups.
    private-link        : Private-link association CLI command group.
    provider            : Manage resource providers.
    redis               : Manage dedicated Redis caches for your Azure applications.
    relay               : Manage Azure Relay Service namespaces, WCF relays, hybrid connections, and
                         rules.
    resource            : Manage Azure resources.
    resourcemanagement  : Resourcemanagement CLI command group.
    rest                : Invoke a custom request.
    restore-point       : Manage restore point with res.
    role                : Manage user roles for access control with Azure Active Directory and
                         service principals.
    search              : Manage Azure Search services, admin keys and query keys.
    security            : Manage your security posture with Microsoft Defender for Cloud.
    servicebus          : Servicebus.
    sf                  : Manage and administer Azure Service Fabric clusters.
    sig                 : Manage shared image gallery.
    signalr             : Manage Azure SignalR Service.
    snapshot            : Manage point-in-time copies of managed disks, native blobs, or other
                         snapshots.
    sql                 : Manage Azure SQL Databases and Data Warehouses.
    sshkey              : Manage ssh public key with vm.
    stack               : A deployment stack is a native Azure resource type that enables you to
                         perform operations on a resource collection as an atomic unit.
    staticwebapp        : Manage static apps.
    storage             : Manage Azure Cloud Storage resources.
    survey              : Take Azure CLI survey.
    synapse             : Manage and operate Synapse Workspace, Spark Pool, SQL Pool.
    tag                 : Tag Management on a resource.
    term                : Manage marketplace agreement with marketplaceordering.
    ts                  : Manage template specs at subscription or resource group scope.
    upgrade             : Upgrade Azure CLI and extensions.
    version             : Show the versions of Azure CLI modules and extensions in JSON format by
                         default or format configured by --output.
    vm                  : Manage Linux or Windows virtual machines.
    vmss                : Manage groupings of virtual machines in an Azure Virtual Machine Scale Set
                         (VMSS).
    webapp              : Manage web apps.

```

## Retrieve Az CLI version installed

Type `az version` in a terminal/powershell/bash window to know what version of the Azure CLI is installed. Your output looks like this:
```json
{
  "azure-cli": "x.xx.0x",
  "azure-cli-core": "x.xx.x",
  "azure-cli-telemetry": "x.x.x",
  "extensions": {}
}
```

![Az-CLI-version][Az-CLI-version]

As we can see, the extensions `"extensions": {}` array is empty in the above output which means we don't have any extensions installed to Az CLI or we can run `az extension list` command to see installed extensions. We can add/install multiple [extensions available][Az CLI extensions list] for Az CLI. Here is detailed explanation of how to [use and manage extensions with the Azure CLI][Use and manage extensions with the Azure CLI].


## Azure CLI endpoints for proxy bypass

If your organization is secured with a firewall or proxy server, you must add certain IP (internet protocol) addresses and domain URLs (uniform resource locators) to the allowlist prior to installing the Azure CLI.

### Public Cloud

### Endpoint

|Endpoint group | Endpoint
|-|-|
management | `https://management.core.windows.net/`
resource_manager | `https://management.azure.com/`
sql_management | `https://management.core.windows.net:8443/`
batch_resource_id | `https://batch.core.windows.net/`
gallery | `https://gallery.azure.com/`
active_directory | `https://login.microsoftonline.com_`
active_directory_resource_id | `https://management.core.windows.net/`
active_directory_graph_resource_id | `https://graph.windows.net/`
microsoft_graph_resource_id | `https://graph.microsoft.com/`
active_directory_data_lake_resource_id | `https://datalake.azure.net/`
vm_image_alias_doc | `https://raw.githubusercontent.com/Azure/azure-rest-api-specs/main/arm-compute/quickstart-templates/aliases.json_`
media_resource_id | `https://rest.media.azure.net_`
ossrdbms_resource_id | `https://ossrdbms-aad.database.windows.net_`
app_insights_resource_id | `https://api.applicationinsights.io_`
log_analytics_resource_id | `https://api.loganalytics.io_`
app_insights_telemetry_channel_resource_id | `https://dc.applicationinsights.azure.com/v2/track_`
synapse_analytics_resource_id | `https://dev.azuresynapse.net_`
attestation_resource_id | `https://attest.azure.net_`
portal | `https://portal.azure.com_`

### Endpoint suffixes

|Suffix name | Suffix
|-|-|
storage_endpoint | *.core.windows.net
storage_sync_endpoint | *.afs.azure.net
keyvault_dns | *.vault.azure.net
mhsm_dns | *.managedhsm.azure.net
sql_server_hostname | *.database.windows.net
mysql_server_endpoint | *.mysql.database.azure.com
postgresql_server_endpoint | *.postgres.database.azure.com
mariadb_server_endpoint | *.mariadb.database.azure.com
azure_datalake_store_file_system_endpoint | *.azuredatalakestore.net
azure_datalake_analytics_catalog_and_job_endpoint | *.azuredatalakeanalytics.net
acr_login_server_endpoint | *.azurecr.io
synapse_analytics_endpoint | *.dev.azuresynapse.net
attestation_endpoint | *.attest.azure.net

### U.S. Government Cloud

### Endpoint

|Endpoint group | Endpoint
|-|-|
management | `https://management.core.usgovcloudapi.net/`
resource_manager | `https://management.usgovcloudapi.net/`
sql_management | `https://management.core.usgovcloudapi.net:8443/`
batch_resource_id | `https://batch.core.usgovcloudapi.net/`
gallery | `https://gallery.usgovcloudapi.net/`
active_directory | `https://login.microsoftonline.us`
active_directory_resource_id | `https://management.core.usgovcloudapi.net/`
active_directory_graph_resource_id | `https://graph.windows.net/`
microsoft_graph_resource_id | `https://graph.microsoft.us/`
vm_image_alias_doc | `https://raw.githubusercontent.com/Azure/azure-rest-api-specs/main/rm-compute/quickstart-templates/aliases.json`
media_resource_id | `https://rest.media.usgovcloudapi.net`
ossrdbms_resource_id | `https://ossrdbms-aad.database.usgovcloudapi.net`
app_insights_resource_id | `https://api.applicationinsights.us`
log_analytics_resource_id | `https://api.loganalytics.us`
app_insights_telemetry_channel_resource_id | `https://dc.applicationinsights.us/v2/track`
synapse_analytics_resource_id | `https://dev.azuresynapse.usgovcloudapi.net`
portal | `https://portal.azure.us`

### Endpoint suffixes

|Suffix name | Suffix
|-|-|
storage_endpoint | *.core.usgovcloudapi.net
storage_sync_endpoint | *.afs.azure.us
keyvault_dns | *.vault.usgovcloudapi.net
mhsm_dns | *.managedhsm.usgovcloudapi.net
sql_server_hostname | *.database.usgovcloudapi.net
mysql_server_endpoint | *.mysql.database.usgovcloudapi.net
postgresql_server_endpoint | *.postgres.database.usgovcloudapi.net
mariadb_server_endpoint | *.mariadb.database.usgovcloudapi.net
acr_login_server_endpoint | *.azurecr.us
synapse_analytics_endpoint | *.dev.azuresynapse.usgovcloudapi.net'

### Azure China Cloud

### Endpoint

|Endpoint group | Endpoint
|-|-|
management | `https://management.core.chinacloudapi.cn/`
resource_manager | `https://management.chinacloudapi.cn`
sql_management | `https://management.core.chinacloudapi.cn:8443/`
batch_resource_id | `https://batch.chinacloudapi.cn/`
gallery | `https://gallery.chinacloudapi.cn/`
active_directory | `https://login.chinacloudapi.cn`
active_directory_resource_id | `https://management.core.chinacloudapi.cn/`
active_directory_graph_resource_id | `https://graph.chinacloudapi.cn/`
microsoft_graph_resource_id | `https://microsoftgraph.chinacloudapi.cn`
vm_image_alias_doc | `https://raw.githubusercontent.com/Azure/azure-rest-api-specs/main/rm-compute/quickstart-templates/aliases.json`
media_resource_id | `https://rest.media.chinacloudapi.cn`
ossrdbms_resource_id | `https://ossrdbms-aad.database.chinacloudapi.cn`
app_insights_resource_id | `https://api.applicationinsights.azure.cn`
log_analytics_resource_id | `https://api.loganalytics.azure.cn`
app_insights_telemetry_channel_resource_id | `https://dc.applicationinsights.azure.cn/v2/rack`
synapse_analytics_resource_id | `https://dev.azuresynapse.azure.cn`
portal | `https://portal.azure.cn`

### Endpoint suffixes

|Suffix name | Suffix
|-|-|
storage_endpoint | *.core.chinacloudapi.cn
keyvault_dns | *.vault.azure.cn
mhsm_dns | *.managedhsm.azure.cn
sql_server_hostname | *.database.chinacloudapi.cn
mysql_server_endpoint | *.mysql.database.chinacloudapi.cn
postgresql_server_endpoint | *.postgres.database.chinacloudapi.cn
mariadb_server_endpoint | *.mariadb.database.chinacloudapi.cn
acr_login_server_endpoint | *.azurecr.cn
synapse_analytics_endpoint | *.dev.azuresynapse.azure.cn

---

## Azure DevOps Extension for Azure CLI

The Azure DevOps Extension for Azure CLI adds Pipelines, Boards, Repos, Artifacts and DevOps commands to the Azure CLI 2.0. You can view the various commands and its usage from [docs.microsoft.com - Azure DevOps Extension Reference][ADO Az commands].

> The Azure CLI with the Azure DevOps Extension has replaced the VSTS CLI. The VSTS CLI has been deprecated and will no longer be receiving new features. Recommendation to users of the VSTS CLI is to switch to Azure CLI and add the Azure DevOps extension. See the [Command Mapping](https://github.com/Azure/azure-devops-cli-extension/blob/master/doc/command_mapping.md) section to view the mapping between VSTS CLI and Azure DevOps Extension commands.
{: .prompt-info }

### Quick start of Installation

1. [Install the Azure CLI](#install-azure-cli). We must have at least `v2.0.69`, which we can verify with `az --version` command. See more info [here](#retrieve-az-cli-version-installed)

2. Add the Azure DevOps Extension by running `az extension add --name azure-devops` in terminal/powershell.

    > We can validate the installation by running `az extension list` or `az version` commands
    ![ADO-Az-CLI-verification][ADO-Az-CLI-verification]
    {: .prompt-info }

3. Run the `az login` command to login into Azure.

   If the CLI can open your default browser, it will do so and load a sign-in page. After successful sign in you might see a page similar to this:
   
   ![Az-Login1][Az-Login1]
   
   Otherwise, you need to open a browser page and follow the instructions on the command line to enter an authorization code after navigating to [https://aka.ms/devicelogin](https://aka.ms/devicelogin) in your browser. For more information, see the [Azure CLI login page](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

> There might be a chance of login failure in terminal if we don't have any subscriptions existed with our Azure account then you might see a similar message:
> ```
> A web browser has been opened at https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize. Please continue the login in the web browser. If no web browser is available or if the web browser fails to open, use device code flow with `az login --use-device-code`.
> The following tenants don't contain accessible subscriptions. Use 'az login --allow-no-subscriptions' to have tenant level access.
> <<Tenant GUID> 'DatOsmic'
> No subscriptions found for <<User Id trying to signing in>.
> ```
>   ![Az-Login2][Az-Login2]
> To fix this issue, as mentioned in the message, please try login with `az login --allow-no-subscriptions` then you will see a JSON output once login is successful from the browser.
{: .prompt-warning }

See the [Get started guide](https://docs.microsoft.com/azure/devops/cli/get-started?view=azure-devops) for detailed setup instructions.

### Usage

```bash
$az [group] [subgroup] [command] {parameters}
```

Adding the Azure DevOps Extension adds `devops`, `pipelines`, `artifacts`, `boards` and `repos` groups.
For usage and help content for any command, pass in the -h parameter, for example:

```bash
$ az devops -h

Group
    az devops : Manage Azure DevOps organization level operations.
        Related Groups
        az pipelines: Manage Azure Pipelines
        az boards: Manage Azure Boards
        az repos: Manage Azure Repos
        az artifacts: Manage Azure Artifacts.

Subgroups:
    admin            : Manage administration operations.
    extension        : Manage extensions.
    project          : Manage team projects.
    security         : Manage security related operations.
    service-endpoint : Manage service endpoints/service connections.
    team             : Manage teams.
    user             : Manage users.
    wiki             : Manage wikis.

Commands:
    configure        : Configure the Azure DevOps CLI or view your configuration.
    feedback         : Displays information on how to provide feedback to the Azure DevOps CLI team.
    invoke           : This command will invoke request for any DevOps area and resource. Please use
                       only json output as the response of this command is not fixed. Helpful docs -
                       https://docs.microsoft.com/en-us/rest/api/azure/devops/.
    login            : Set the credential (PAT) to use for a particular organization.
    logout           : Clear the credential for all or a particular organization.
```

## Authentication

If you would have followed above mentioned steps, then you are authenticated using [User prompted to use az devops login][User prompted to use az devops login] but we can [Sign in with a personal access token (PAT)][Sign in with a personal access token (PAT)]. You can sign in using an Azure DevOps personal access token (PAT). To create a PAT, see [Use personal access tokens][Use personal access tokens].

To use a PAT with the Azure DevOps CLI, use one of these options:

* Use `az devops login` and be [prompted for the PAT token][prompted for the PAT token].
* Pipe the [PAT token on StdIn][PAT token on StdIn] to `az devops login`. 

  > This option works only in a non-interactive shell.
  {: .prompt-info }

* Set the `AZURE_DEVOPS_EXT_PAT` [environment variable][environment variable], and don't use `az devops login`.


## Configure Organization and Project settings

Configuring default settings are recommend, to set the default configuration for your organization and project. Otherwise, we can set these within the individual commands themselves and we can use `--detect` to automatically detect organization while running specific commands instead of providing in every command.To set default configuration:
```
az devops configure --defaults organization=<<OrganizationName>> project=<<ProjectName>>
```
Example: `az devops configure --defaults organization=https://dev.azure.com/contoso project=ContosoWebApp`

> We can get these organization and project names from the URL `https://dev.azure.com/datosmic/datosmic-project1` directly. Here `datosmic` is the **Organization Name** and `datosmic-project1` is the **Project Name**.
{: .prompt-tip }

## Retrieve Project details using Az CLI

Run the *[az devops project list][az devops project list]* command to get the list of projects under Organization.
```bash
az devops project list [--continuation-token]
                       [--detect {false, true}]
                       [--get-default-team-image-url {false, true}]
                       [--org]
                       [--skip]
                       [--state-filter {all, createPending, deleted, deleting, new, unchanged, wellFormed}]
                       [--top]
```

You will see an output looks like:
```json
{
  "continuationToken": null,
  "value": [
    {
      "abbreviation": null,
      "defaultTeamImageUrl": null,
      "description": "datosmic-project1",
      "id": "03041377-d072-4a85-a266-XXXXxxxxXXXX",
      "lastUpdateTime": "2024-01-26T14:03:46.803000+00:00",
      "name": "datosmic-project1",
      "revision": 21,
      "state": "wellFormed",
      "url": "https://dev.azure.com/datosmic/_apis/projects/03041377-d072-4a85-a266-XXXXxxxxXXXX",
      "visibility": "private"
    }
  ]
}
```

<!-- Reference Images -->
[Azure-CLI-Installation]: /assets/img/2024-01-26-Azure-DevOps-CLI/Azure-CLI-Installation.png
[Azure-CLI-Installation2]: /assets/img/2024-01-26-Azure-DevOps-CLI/Azure-CLI-Installation2.png
[Az-CLI-version]: /assets/img/2024-01-26-Azure-DevOps-CLI/Az-CLI-version.png
[ADO-Az-CLI-verification]: /assets/img/2024-01-26-Azure-DevOps-CLI/ADO-Az-CLI-verification.png
[Az-Login1]: /assets/img/2024-01-26-Azure-DevOps-CLI/Az-Login1.png
[Az-Login2]: /assets/img/2024-01-26-Azure-DevOps-CLI/Az-Login2.png

<!-- Reference Links -->
[Az CLI]: https://learn.microsoft.com/en-us/cli/azure/what-is-azure-cli
[Install Az CLI]: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli
[Install Az CLI-Win]: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows
[Install Az CLI-Linux]: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux
[Install Az CLI-Mac]: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-macos
[Az CLI extensions list]: https://aka.ms/azure-cli-extension-index-v1
[Use and manage extensions with the Azure CLI]: https://learn.microsoft.com/en-us/cli/azure/azure-cli-extensions-overview
[ADO Az commands]: https://docs.microsoft.com/en-us/cli/azure/devops?view=azure-cli-latest
[A-Z Az CLI]: https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest
[User prompted to use az devops login]: https://learn.microsoft.com/en-us/azure/devops/cli/log-in-via-pat?view=azure-devops&tabs=windows#user-prompted-to-use-az-devops-login
[Sign in with a personal access token (PAT)]: https://learn.microsoft.com/en-us/azure/devops/cli/log-in-via-pat?view=azure-devops&tabs=windows
[Use personal access tokens]: https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?toc=%2Fazure%2Fdevops%2Forganizations%2Ftoc.json&view=azure-devops
[prompted for the PAT token]: https://learn.microsoft.com/en-us/azure/devops/cli/log-in-via-pat?view=azure-devops&tabs=windows#userprompt
[PAT token on StdIn]: https://learn.microsoft.com/en-us/azure/devops/cli/log-in-via-pat?view=azure-devops&tabs=windows#PipePATonStdIn
[environment variable]: https://learn.microsoft.com/en-us/azure/devops/cli/log-in-via-pat?view=azure-devops&tabs=windows#EnvironmentVariable
[az devops project list]: https://learn.microsoft.com/en-us/cli/azure/devops/project?view=azure-cli-latest#az-devops-project-list