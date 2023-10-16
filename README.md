# Azure Arc Enabled Servers
## Dashboard to track Extended Security Updates (ESU) for Window 2012 and SQL 2012

Extended Security Updates (ESUs) can be delivered to Windows Server 2012 and SQL Server 2012 systems that are not hosted in Azure by registering those systems with Azure Arc Enabled Server and assigning an ESU license to them.

Although there is some visibility available in the Azure Portal relating to ESU licenses, you may want to create your own custom dashboard to track their assigment.

In this simple example, the Azure Resource Graph and the Azure Portal Dashboard are used to build a custom view. It helps track which Arc Enabled Servers are running Windows and SQL versions that may be eligible for ESU, and if an ESU license has been assigned.

![screenshot]

The Azure Portal Dashboard is made up of a number of Azure Resource Graph queries such as this example below which is used to count the number of Arc Enabled Windows Server 2012 computers where an ESU license is not yet assigned.

```KQL
resources 
| where ['type'] =~ 'Microsoft.HybridCompute/machines'
| where properties.osSku contains 'Windows Server 2012'
| where properties.licenseProfile.esuProfile.licenseAssignmentState == 'NotAssigned'
| project esuState=properties.licenseProfile.esuProfile.licenseAssignmentState
| count
```

## Deployment steps

First download the dashboard template JSON file located in the 'templates' folder to a folder on your local device. Login to the Azure Portal and navigate to **Dashboard**. Once in the Dashboard view, select **Upload** and locate the dashboard template you just downloaded.

![upload]

## Reference
- For more information about delivering ESU using Azure Arc Enabled Servers, refer to: [How to get Extended Security Updates (ESU) for Windows Server][esuDocs].
- To learn more about using the Azure Resource Graph, refer to: [What is Azure Resource Graph?][azureResourceGraphDocs]

`Tags: Extended Security Updates, Azure Resource Graph, Azure Dashboard`

<!-- References -->

<!-- Local -->
[screenshot]: images/screenshot.png
[upload]: images/uploadTemplate.png

<!-- External -->
[esuDocs]: https://learn.microsoft.com/en-us/azure/azure-arc/servers/deliver-extended-security-updates?toc=%2Fwindows-server%2Fget-started%2Ftoc.json&bc=%2Fwindows-server%2Fbreadcrumbs%2Ftoc.json
[azureResourceGraphDocs]: https://learn.microsoft.com/en-us/azure/governance/resource-graph/overview
