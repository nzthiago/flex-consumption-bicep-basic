---
description: This Bicep sample deploys the resources to create a function app in Azure Functions that runs in a Flex Consumption plan.
page_type: sample
products:
- azure
- azure-resource-manager
urlFragment: bicep-file-deployment
languages:
- bicep
---

# Flex Consumption plan - Basic Bicep samples | Azure Functions

> [!WARNING]
> Note: some of the examples in this repo use connection strings and don't use virtual networking to secure storage. For security best practices with managed identity and networking follow [these samples](https://github.com/Azure-Samples/azure-functions-flex-consumption-samples) instead.

The bicep samples in this repo deploy a Flex Consumption function app with associated resources like Application Insights and storage accounts. They create these Azure components:

| Component | Description |
| ---- | ---- |
| **Function app** | This is the serverless Flex Consumption app where you can deploy your functions code. The function app is configured with Application Insights and Storage Account.|
| **Function app plan** | The Azure Functions app plan associated with your Flex Consumption app. For Flex Consumption there is only one app allowed per plan, but the plan is still created.|
| **Application Insights** | This is the telemetry service associated with the Flex Consumption app for you to monitor live applications, detect performance anomalies, review telemetry logs, and to understand your app behavior.|
| **Log Analytics Workspace** | This is the workspace used by Application Insights for the app telemetry.|
| **Storage Account** | This is the Microsoft Azure storage account that [Azure Functions requires](https://learn.microsoft.com/azure/azure-functions/storage-considerations) when you create a function app instance.|

## List of samples

* **basic-onefile-managedidentity** - this folder has one-file bicep file that creates the function app and uses managed identity to connect to Azure Storage and App Insights.
* **basic-withsubfolders-connstrings** - this folder has a bicep app with subfolder modules that creates the function app and uses connection strings to connect to Azure Storage. This is not recommended, use managed identity instead.

## How to deploy it?

Use these steps to deploy using the Bicep file.

### 1. Modify the parameters file

In the folder of the sample you want to deploy, create a copy and modify the parameters file `main.bicepparam` to specify the values for the parameters. The parameters file contains the following parameters that you must specify values for before you can deploy the app:

| Parameter | Description |
| ---- | ---- |
| **environmentName** | a unique name to be used for the resources being created.|
| **location** | the location where the assets will be created. You can find the supported regions with the `az functionapp list-flexconsumption-locations` command of the Azure CLI.|

Here is an example `main.bicepparam` that you can modify:

```bicep
using 'main.bicep'
param environmentName = 'myflexconsumptionapp'
param location = 'eastus'
```

### 2. Deploy the bicep file

Before you can deploy this app, you need a way to deploy Bicep files. For example, you can use [Visual Studio Code with the Bicep extension](https://learn.microsoft.com/azure/azure-resource-manager/bicep/deploy-vscode), the [Azure CLI](https://learn.microsoft.com/azure/azure-resource-manager/bicep/deploy-cli) (make sure you have the latest version of the Bicep CLI installed by running `az bicep upgrade`), or [PowerShell](https://learn.microsoft.com/azure/azure-resource-manager/bicep/deploy-powershell).

For example, if you created a `maincopy.bicepparam` file with the above parameter values for the **basic-onefile-managedidentity** sample, you can deploy the app by running the following command, making sure to modify the value for location with the same location as in your updated copy of `main.bicepparam`:

```bash
cd basic-onefile-managedidentity
az deployment sub create --name fcthibicep1 --location eastus --template-file main.bicep --parameters maincopy.bicepparam
```

Once deployed you should see the services created on Azure:
![Resources described above in the resource group](resources.png)

You can now use the [Azure Functions Core Tools](https://learn.microsoft.com/azure/azure-functions/functions-run-local), [VS Code](https://learn.microsoft.com/azure/azure-functions/functions-develop-vs-code), or [Visual Studio](https://learn.microsoft.com/azure/azure-functions/functions-develop-vs?pivots=isolated), to create and publish your app code to the function app.
