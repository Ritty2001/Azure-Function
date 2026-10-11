## HTTP-triggered Azure Function + Blob Storage

What we are building: An HTTP-triggered function that receives a name via query string, generates a text file, and saves it to Blob Storage.

How we are doing it:
We create a resource group and storage account
Create the function app 
Program a function in the function app which automatically creates a text file in a specified blob container when you give a name in the query HTTP String. We are using a method called “core tools” which is a module that must be installed in our local system that helps us code Azure functions.
We need to create a blob container and specify it so the Azure function knows which blob the text file needs to be created under.
We deploy the function app in Azure.
Then we give a CURL command to test the deployed version with the below URL. The API key can be obtained by the command mentioned in the steps in the later part of the doc.
Eg. When we query: "https://func-demo-app<unique>.azurewebsites.net/api/HttpToBlob?code=<function-key>&name=Sample"
The word “Sample” Mentioned in bold will be the name of the Blob container created by the function.

Steps

Login to azure in your terminal using az login and input the credentials

Create the resource group + storage account (Replace <unique> with random string of characters since storage account names are globally unique or just use any random globally unique name)
      az group create --name func-demo-rg --location eastus
      az storage account create --name funcdemostorage<unique> --location eastus --         resource-group func-demo-rg --sku Standard_LRS

Create the Function App (Replace <unique> with random string of characters since Function App names are globally unique or just use any random globally unique name). We are creating a python function with runtime version 3.11

az functionapp create --resource-group func-demo-rg --consumption-plan-location eastus \
  --runtime python --runtime-version 3.11 --functions-version 4 \
  --name func-demo-app<unique> --storage-account funcdemostorage<unique>

3. Write the HTTP trigger function locally using Core Tools
Install Core Tools (if not already)
npm install -g azure-functions-core-tools@4 --unsafe-perm true

Verify:
Func –version

Initialize the project
func init FuncDemoProject --python
cd <foldername>

Check local.settings.json — v2 projects should have:
"AzureWebJobsFeatureFlags": "EnableWorkerIndexing"
This flag is what tells Functions to use the decorator-based v2 model. If it's missing, add it.
You'll now have a single function_app.py at the root instead of per-function folders.
3. Create the HTTP trigger function
func new --name HttpToBlob --template "HTTP trigger" --authlevel "function"
It appends a decorated function block directly into function_app.py.

Write the function with a Blob output binding (decorator-based)
This function code is available at function_app.py

5. Create the container it writes to
az storage container create --name demo-container --account-name funcdemostorage<unique>


6. Deploy the function App
func azure functionapp publish func-demo-app<unique>

Get the key:
az functionapp function keys list --name func-demo-app<unique> --resource-group func-demo-rg --function-name HttpToBlob

8. Test the deployed version
curl.exe "https://func-demo-app<unique>.azurewebsites.net/api/HttpToBlob?code=<function-key>&name=Sample"

This will create a blob called “Sample” in the created storage container in that particular storage account.




