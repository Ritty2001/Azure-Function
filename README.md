HTTP-triggered Azure Function + Blob Storage

What we are building: An HTTP-triggered function that receives a name via query string, generates a text file, and saves it to Blob Storage.

How we are doing it:
We create a resource group and storage account
Create the function app 
Program a function in the function app which automatically creates a text file in a specified blob container when you give a name in the query HTTP String. We are using a method called “core tools” which is a module that must be installed in our local system that helps us code Azure functions.
We need to create a blob container and specify it so the Azure function knows which blob the text file needs to be created under.
We deploy the function app in Azure.
Then we give a CURL command to test the deployed version with the below URL. The API key can be obtained by the command mentioned in the steps in the later part of the doc.
Eg: "https://func-demo-app<unique>.azurewebsites.net/api/HttpToBlob?code=<function-key>&name=Sample"
The word “Sample” Mentioned in bold will be the name of the Blob container created by the function.
