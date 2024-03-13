# Lab 01 - Add parameters and variables to your Bicep template

## Lab Scenario

## Lab objectives

## Estimated timing: 60 minutes

### Exercise - Add parameters and variables to your Bicep template

In this exercise, you will

#### Task 1: Add the location and resource name parameters

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
