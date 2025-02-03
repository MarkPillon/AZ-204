# Azure Functions Basics - Step by step Lab Guide

### Objective:

To learn how to create, configure, and test an Azure Function using JavaScript, including settign up input and output Bindings with Azure Blob Storage.

# Step 1: Create a Function App

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

# Step 2: 