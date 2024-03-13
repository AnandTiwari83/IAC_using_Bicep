# Lab 01 - Define resources in a Bicep template

## Lab Scenario

## Lab objectives

## Estimated timing: 60 minutes

### Exercise 1: Create a Bicep template that contains a storage account

In this exercise, you will 

### Task 1: Open the Azure portal

1. Open Visual Studio Code.

1. Create a new file called **main.bicep**.

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

### Task 2: Deploy the Bicep template to Azure

1. On the Terminal menu, select New Terminal. The terminal window usually opens in the lower half of your screen.
1. If the shell shown on the right side of the terminal window is powershell or pwsh, the correct shell is open, and you can skip to the next section.

    ![](./media/pwsh.png)
   
1. If a shell other than powershell or pwsh appears, select the shell dropdown arrow, and then select PowerShell.
1. In the list of terminal shells, select powershell or pwsh.
1. In the terminal, go to the directory where you saved your template. For example, if you saved your template in the templates folder, you can use this command:

   ```
   Set-Location -Path templates
   ```
### Task 3: Sign in to Azure by using Azure PowerShell

1. In the Visual Studio Code terminal, run the following command:

   ```
   Connect-AzAccount
   ```
   **Note**: A browser opens so that you can sign in to your Azure account.
1. After you've signed in to Azure, the terminal displays a list of the subscriptions associated with this account.
1. Set the default subscription for all of the Azure PowerShell commands that you run in this session.

   ```
   $context = Get-AzSubscription -SubscriptionName 'Concierge Subscription'
   Set-AzContext $context
   ```
1. Get the subscription ID. Running the following command lists your subscriptions and their IDs. Look for Concierge Subscription, and then copy the ID from the second column. It looks something like cf49fbbc-217c-4eb6-9eb5-a6a6c68295a0.

   ```
   Get-AzSubscription
   ```
1. Change your active subscription to Concierge Subscription. Be sure to replace {Your subscription ID} with the one that you copied.

   ```
   $context = Get-AzSubscription -SubscriptionId {Your subscription ID}
   Set-AzContext $context
   ```
1. Deploy the template to Azure by using the following Azure PowerShell command in the terminal. The command can take a minute or two to complete, and you'll see a successful deployment. If you see a warning about the location being hard-coded, you can ignore it. You'll fix the location later in the module. It's safe to proceed and the deployment will succeed.

   ```
   New-AzResourceGroupDeployment -TemplateFile main.bicep
   ```

### Task 4: Verify the deployment
The first time you deploy a Bicep template, you might want to use the Azure portal to verify that the deployment has finished successfully and to inspect the results.

1. 1. If you are not logged in already, click on the **Azure portal** shortcut that is available on the desktop and log in with Azure credentials.

1. If not Sign-in, then on the **Sign into Microsoft Azure** tab you will see the login screen, in that enter the following **Email/**Username** and then click on **Next**. 
   * Email/Username: <inject key="AzureAdUserEmail"></inject>
   
    ![](https://github.com/CloudLabsAI-Azure/AIW-SAP-on-Azure/raw/main/media/M2-Ex1-portalsignin-1.png?raw=true)
    
1. Now enter the following **Password** and click on **Sign in**.
   * Password: <inject key="AzureAdUserPassword"></inject>

    ![](./media/M2-Ex1-portalsignin-(2).png)
    
1. If you see the pop-up **Stay Signed in?**, click No.

    ![](./media/staysignedinNO1.png)

1. On the left-side panel, select Resource groups.

1. Select [sandbox resource group name].

2. In Overview, you can see that one deployment succeeded. You might need to expand the Essentials area to see the deployment.

3. Select 1 Succeeded to see the details of the deployment.

4. Select the deployment called main to see which resources were deployed, then select Deployment details to expand it. In this case, there's one storage account with the name that you specified.
5. Leave the page open in your browser. You'll check on deployments again later.

### Task 5: Add an App Service plan and app to your Bicep template
In the previous task, you learned how to create a template that contains a single resource and deploy it. Now you're ready to deploy more resources, including a dependency. In this task, you'll add an App Service plan and app to the Bicep template.

1. In the main.bicep file in Visual Studio Code, add the following code to the bottom of the file:

   ```
     resource appServicePlan 'Microsoft.Web/serverFarms@2022-03-01' = {
       name: 'toy-product-launch-plan-starter'
       location: 'westus3'
       sku: {
         name: 'F1'
       }
     }
     
     resource appServiceApp 'Microsoft.Web/sites@2022-03-01' = {
       name: 'toy-product-launch-1'
       location: 'westus3'
       properties: {
         serverFarmId: appServicePlan.id
         httpsOnly: true
       }
     }
   ```
1. Update the name of the App Service app from toy-product-launch-1 to something that's likely to be unique. Make sure the name is 2 to 60 characters with uppercase and lowercase letters, numbers, and hyphens, and doesn't start or end with a hyphen.
2. Save the changes to the file.

### Task 6: Deploy the updated Bicep template

1. Run the following Azure PowerShell command in the terminal. You can ignore the warning messages about the hard-coded location. You'll fix the location soon.

   ```
   New-AzResourceGroupDeployment -TemplateFile main.bicep
   ```
1. Return to the Azure portal and go to your resource group. You'll still see one successful deployment, because the deployment used the same name as the first deployment.
2. Select the 1 Succeeded link.
3. Select the deployment called main, and then select Deployment details to expand the list of deployed resources.
4. Notice that the App Service plan and app were deployed.
