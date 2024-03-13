# Lab 01 - Add parameters and variables to your Bicep template

## Lab Scenario

## Lab objectives

## Estimated timing: 60 minutes

### Exercise - Add parameters and variables to your Bicep template

In this exercise, you will

### Task 1: Add the location and resource name parameters

1. In the main.bicep file in Visual Studio Code, add the following code to the top of the file:

    ```
    param location string = 'westus3'
    param storageAccountName string = 'toylaunch${uniqueString(resourceGroup().id)}'
    param appServiceAppName string = 'toylaunch${uniqueString(resourceGroup().id)}'

    var appServicePlanName = 'toy-product-launch-plan'
    ```

1. Find the places within the resource definitions where the location and name properties are set, and update them to use the parameter values. After you're finished, the resource definitions within your Bicep file should look like this:

    ```
    resource storageAccount 'Microsoft.Storage/storageAccounts@2022-09-01' = {
      name: storageAccountName
      location: location
      sku: {
        name: 'Standard_LRS'
      }
      kind: 'StorageV2'
      properties: {
        accessTier: 'Hot'
      }
    }
    
    resource appServicePlan 'Microsoft.Web/serverFarms@2022-03-01' = {
      name: appServicePlanName
      location: location
      sku: {
        name: 'F1'
      }
    }
    
    resource appServiceApp 'Microsoft.Web/sites@2022-03-01' = {
      name: appServiceAppName
      location: location
      properties: {
        serverFarmId: appServicePlan.id
        httpsOnly: true
      }
    }
    ```

1. Save the changes to the file.

### Task 2: Automatically set the SKUs for each environment type

1. In the main.bicep file in Visual Studio Code, add the following Bicep parameter below the parameters that you created in the previous task:

    ```
    @allowed([
      'nonprod'
      'prod'
    ])
    param environmentType string
    ```

1. Below the line that declares the appServicePlanName variable, add the following variable definitions:

    ```
    var storageAccountSkuName = (environmentType == 'prod') ? 'Standard_GRS' : 'Standard_LRS'
    var appServicePlanSkuName = (environmentType == 'prod') ? 'P2v3' : 'F1'
    ```

1. Find the places within the resource definitions where the sku properties are set and update them to use the parameter values. After you're finished, the resource definitions in your Bicep file should look like this:

    ```
    resource storageAccount 'Microsoft.Storage/storageAccounts@2022-09-01' = {
      name: storageAccountName
      location: location
      sku: {
        name: storageAccountSkuName
      }
      kind: 'StorageV2'
      properties: {
        accessTier: 'Hot'
      }
    }
    
    resource appServicePlan 'Microsoft.Web/serverFarms@2022-03-01' = {
      name: appServicePlanName
      location: location
      sku: {
        name: appServicePlanSkuName
      }
    }
    
    resource appServiceApp 'Microsoft.Web/sites@2022-03-01' = {
      name: appServiceAppName
      location: location
      properties: {
        serverFarmId: appServicePlan.id
        httpsOnly: true
      }
    }
    ```

1. Save the changes to the file.

#### Task 2.1: Verify your Bicep file

1. After you've completed all of the preceding changes, your main.bicep file should look like this example:

    ```
    param location string = 'westus3'
    param storageAccountName string = 'toylaunch${uniqueString(resourceGroup().id)}'
    param appServiceAppName string = 'toylaunch${uniqueString(resourceGroup().id)}'
    
    @allowed([
      'nonprod'
      'prod'
    ])
    param environmentType string
    
    var appServicePlanName = 'toy-product-launch-plan'
    var storageAccountSkuName = (environmentType == 'prod') ? 'Standard_GRS' : 'Standard_LRS'
    var appServicePlanSkuName = (environmentType == 'prod') ? 'P2v3' : 'F1'
    
    resource storageAccount 'Microsoft.Storage/storageAccounts@2022-09-01' = {
      name: storageAccountName
      location: location
      sku: {
        name: storageAccountSkuName
      }
      kind: 'StorageV2'
      properties: {
        accessTier: 'Hot'
      }
    }
    
    resource appServicePlan 'Microsoft.Web/serverFarms@2022-03-01' = {
      name: appServicePlanName
      location: location
      sku: {
        name: appServicePlanSkuName
      }
    }
    
    resource appServiceApp 'Microsoft.Web/sites@2022-03-01' = {
      name: appServiceAppName
      location: location
      properties: {
        serverFarmId: appServicePlan.id
        httpsOnly: true
      }
    }
    ```
    > Note: If your file doesn't match, copy the example or adjust your file to match the example.

#### Task 2.2: Deploy the updated Bicep template

1. Run the following Azure PowerShell command in the terminal.

    ```
    New-AzResourceGroupDeployment -TemplateFile main.bicep -environmentType nonprod
    ```

### Task 3: Check your deployment

1. In your browser, go back to the Azure portal and go to your resource group. You'll still see one successful deployment, because the deployment used the same name as the first deployment.

1. Select the 1 Succeeded link.

1. Select the deployment called main, and then select Deployment details to expand the list of deployed resources.

    ![](./media/6-addparams-details.png)

1. Notice that a new App Service app and storage account have been deployed with randomly generated names.

