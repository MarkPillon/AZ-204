# Azure Functions Basics - Step by step Lab Guide

### Objective:

To learn how to create, configure, and test an Azure Function using JavaScript, including settign up input and output Bindings with Azure Blob Storage.

## Step 1: Create a Function App

### 1.1 Navigate to Azure Portal

- Log in to Azure Portal.
- Search for 'Function App' in the top seach bar

### 1.2 Create a new Function App

- Click on 'Create'
- Fill in the following details:
    - **Subscription**: Choose your appropriate Azure subscription.
    - **Resource Group**: Create or select an existing resource group.
    - **Function App Name**: Enter a globally unique name --> ```my-first-function-app```.
    - **Publish**: Choose 'Code'.
    - **Runtime Stack**: Select 'JavaScript'
    - **Version**: Select '4' (Ensure that you're using Azure Function v4)
    - **Region**: Select a suitable region for your application 
    - **Operating System**: Choose 'Windows' or 'Linux' 
    - **Hosting Plan**: Choose 'Consumption (Serverless)'

### 1.3 Review and Create 

- Click 'Review + Create', then click 'Create' after the validation process in completed.

## Step 2: Set up Visual Studio Code for Azure Function 

### 2.1 Install Visual Studio Code adn Required Extensions

- Install [Visual Studio Code](https://code.visualstudio.com/download)
- Install the following extensions:
    - Azure Functions 
    - Azure Functions Core Tools v4

### 2.2 Configure Environment

- **GitPod**: Optionally use [GitPod](https://www.gitpod.io/) to launch a Linux-based development environment.
- **Local Setup**: Alternatively, use a local Visual Studio Code setup with Windows Subsystem for Linux (WSL).

## Step 3: Create a New Function 

### 3.1 Create a New Function 

- Open Visual Studio Code
- Go to the Azure Functions extension on the left-hand side.
- Click 'Create New Project'/
- Choose **JavaScript** as the language
- **Function Template**: Choose 'HTTP Trigger' as the template.
- Enter a name for the function (such as, ```httpTriggerFunction```)
- Choose authorization level as 'Function'.

### 3.2 Customize the Code

- Open the generated ```index.js``` file.
- Customize the following code:

```js
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));
    context.res = {
        // status: 200, /* Defaults to 200 */
        body: name ? `Hello, ${name}` : "Pass a name in the query string or in the request body"
    };
}
```

### 3.3 Deploy the Function

- Click on 'Deplpoy to Function App' in the Azure Functions extension 
- Select the **Function App** you created earlier  (```httpTriggerFunction```)
- Confirm the deployment.

## Step 4: Set up INput and Output Bindings

### 4.1 Create Azure Storage Account Containers

- Go to the **Storage Accout** associated with your Function App.
- Create COntainers:
    - In-Container: For input.
    - Out-Container: For output.

### 4.2 Add Bindings to the Function 

- Open the ```function.json``` file in your project.
- Add the following input and output bindings.

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["get", "post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "blob",
      "direction": "in",
      "name": "myInputBlob",
      "path": "in-container/{name}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "blob",
      "direction": "out",
      "name": "myOutputBlob",
      "path": "out-container/{name}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

### 4.3 Update the Function Code for Bindings

- Modify the function code to process input and output.

```js
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    const name = req.query.name || (req.body && req.body.name);

    // Use input binding
    const inputData = context.bindings.myInputBlob;

    // Set output binding
    context.bindings.myOutputBlob = inputData;

    context.res = {
        body: name ? `Hello, ${name}` : "Pass a name in the query string or in the request body"
    };
}
```

### 4.4 Deploy the Function 

- Deploy the updated function by clicking 'Deploy to Function App' again 

## Step 5 Testing the Function 

### 5.1 Run the Function Locally 

- Run the function using the **Azure Functions Core Tools** command.

```sh
func start
```

- Open a new terminal and test using ```curl```:

```sh
curl -X GET "http://localhost:7071/api/httpTriggerFunction?name=world"
```

### 5.2 Test on Azure

- Go to your Function App in the Azure Portal.
- **Navigate to the Function**: Select your function ```httpTriggerFunction```.
- click 'Get Function URL' and copy the URL.
- Append the query string:

```sh
?name=data
```

- run the function in the browser or using ```curl```.

### 5.3 Check Blob Storage

- **Verify Output**: Go to the Out-Container in Azure Storage.
Confirm that the file has been copied from the In-Container.

## Step 6 Cleanup Resources 

### 6.1 Delete Resource Group

- Go to **Resource Gruoup** in the Azure Portal.
- Select the **Resource Group** you created.
- Click 'Delete Resource Group' to delete all associated resources.


### Notes for Azure Functions v4

- **Version Upgrade**: If you are migrating from Azure Functions v3 to v4, make sure to refer to the [migration guide](https://learn.microsoft.com/en-us/azure/azure-functions/migrate-version-3-version-4?tabs=net8%2Cazure-cli%2Cwindows&pivots=programming-language-javascript) for steps required to upgrade your functions and runtime setttings.

- **Bindings**: The syntax and features in Azure Functions v4 might has slight changves compared to v3.  Refer to the [v4 documentation](https://learn.microsoft.com/en-us/azure/azure-functions/create-first-function-cli-node?tabs=windows%2Cazure-cli%2Cbrowser&pivots=nodejs-model-v4) for specific configuration detals.

- **Deployment Considerations**: Use Azure Functions Core Tools v4 for local testing to ensure compatibility with the v4 runtime on Azure.

