# Lab 01 - Define resources in a Bicep template

## Lab Scenario

## Lab objectives

## Estimated timing: 60 minutes

### Exercise 1: Create a Bicep template that contains a storage account

In this exercise, you will 

#### Task 1: Open the Azure portal

1. Open Visual Studio Code.

1. Create a new file called main.bicep.

1. Save the empty file so that Visual Studio Code loads the Bicep tooling.

1. Add the following Bicep code into the file. You'll deploy the template soon. It's a good idea to type the code yourself instead of copying and pasting, so you can see how the tooling helps you to write your Bicep files.

     ```
    resource storageAccount 'Microsoft.Storage/storageAccounts@2022-09-01' = {
      name: 'toylaunchstorage'
      location: 'westus3'
      sku: {
        name: 'Standard_LRS'
      }
      kind: 'StorageV2'
      properties: {
        accessTier: 'Hot'
      }
    }
    ```
    **Note**: Notice that Visual Studio Code automatically suggests property names as you type. The Bicep extension for Visual Studio Code understands the resources you're defining in your template, and it lists the available properties and values that you can use.

1. Update the name of the storage account from toylaunchstorage to something that's likely to be unique, because every storage account needs a globally unique name. Make sure the name is 3 to 24 characters and includes only lowercase letters and numbers.

1. Save the changes to the file.

#### Task 2: Deploy the Bicep template to Azure

1. On the Terminal menu, select New Terminal. The terminal window usually opens in the lower half of your screen.
2. If the shell shown on the right side of the terminal window is powershell or pwsh, the correct shell is open, and you can skip to the next section.
3. If a shell other than powershell or pwsh appears, select the shell dropdown arrow, and then select PowerShell.
4. In the list of terminal shells, select powershell or pwsh.
5. In the terminal, go to the directory where you saved your template. For example, if you saved your template in the templates folder, you can use this command:

   ```
   Set-Location -Path templates
   ```
