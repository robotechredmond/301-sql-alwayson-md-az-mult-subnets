# Create a SQL Server 2016 AlwaysOn Availability Group in an existing Azure VNET and Active Directory domain across AZ's

This template will create a SQL Server 2016 AlwaysOn Availability Group using the PowerShell DSC Extension in an existing Azure Virtual Network and Active Directory environment. The SQL Server VMs will be provisioned across multiple Azure AZ's.

This template creates the following resources:

+	One Storage Account for Cloud Witness
+	Two-to-Nine SQL Server 2016 VMs in a Windows Server 2016 Cluster with Managed Disks

A SQL Server AlwaysOn Listener is created using secondary private IP addresses on each NIC.

To deploy the required Azure VNET and Active Directory infrastructure, if not already in place, you may use <a href="https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc">this template</a> to deploy the prerequisite infrastructure.

## Notes

+	The default settings for storage are to deploy using **premium storage**.  The SQL VMs use two P30 disks each (for data and log).  These sizes can be changed by changing the relevant variables. In addition there is a P10 Disk used for each VM OS Disk.

+ 	The default settings for compute require that you have at least 4 cores of free quota to deploy.

+ 	The images used to create this deployment are
	+ 	SQL Server - Latest SQL Server 2016 on Windows Server 2016 Image

+ 	The image configuration is defined in variables, but the scripts that configure this deployment have only been tested with these versions and may not work on other images.

+	To successfully deploy this template, be sure that the subnet to which the SQL VMs are being deployed already exists on the specified Azure virtual network, AND this subnet should be defined in Active Directory Sites and Services for the appropriate AD site in which the closest domain controllers are configured.

Click the button below to deploy from the portal

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Frobotechredmond%2F301-sql-alwayson-md-az%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Frobotechredmond%2F301-sql-alwayson-md-az%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

## Deploying Sample Templates

You can deploy these samples directly through the Azure Portal or by using the scripts supplied in the root of the repo.

To deploy a sammple using the Azure Portal, click the **Deploy to Azure** button found in the README.md of each sample.

To deploy the sample via the command line (using [Azure PowerShell or the Azure CLI](https://azure.microsoft.com/en-us/downloads/)) you can use the scripts.

Simple execute the script and pass in the folder name of the sample you want to deploy.  For example:

```PowerShell
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation 'eastus' -ArtifactsStagingDirectory '[foldername]'
```
```bash
azure-group-deploy.sh -a [foldername] -l eastus -u
```
If the sample has artifacts that need to be "staged" for deployment (Configuration Scripts, Nested Templates, DSC Packages) then set the upload switch on the command.
You can optionally specify a storage account to use, if so the storage account must already exist within the subscription.  If you don't want to specify a storage account
one will be created by the script or reused if it already exists (think of this as "temp" storage for AzureRM).

```PowerShell
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation 'eastus' -ArtifactsStagingDirectory '201-vm-custom-script-windows' -UploadArtifacts 
```
```bash
azure-group-deploy.sh -a '201-vm-custom-script-windows' -l eastus -u
```
Tags: ``cluster, ha, sql, alwayson``