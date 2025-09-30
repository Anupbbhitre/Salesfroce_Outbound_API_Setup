# Salesfroce_Outbound_API_Setup

This repository contains Salesforce Apex code and  for integrating with the backend  AWS API to fetch and update Bank Information (Bank_Information__c) when a Routing Number is updated.

When a Routing Number changes in Salesforce, an Apex callout is triggered.

Salesforce makes a REST call to the Wire AWS API with the updated Routing Number.

The API response returns bank details and address, which are updated back into the Bank_Information__c record.

Authentication is handled with an Access Token (stored in a custom object) and environment credentials (stored in custom settings).

⚙️ Architecture

Trigger / Handler

A trigger on Bank_Information__c listens for changes to the Wire_Routing_Number__c field.

Internal Plateform Event(for Asyncronous calling)

Calls the Apex service class to make an API request.

Service Layer (Callout Class)

Handles the REST callout to AWS Wire API.

Uses the stored access token for authentication.

Refreshes token if expired (using custom object storage).

Response Processing

Extracts Bank Name, Address, City, State, Postal Code (and other available fields) from the response.

Updates the same Bank_Information__c record in Salesforce.

Token Management

Custom Object (e.g., Wire_API_Token__c) → Stores Access_Token, Expiry, and related info.

Custom Setting (e.g., Wire_API_Config__c) → Stores environment-level details:

API Base URL (Sandbox / Production)

Client ID / Client Secret

Auth URL

/force-app/main/default
  ├── classes/
  │    ├── BankAddresHandler.cls        # Main callout logic
  |    ├── BankAddresHandlerTest.cls
  │    ├── AwsAccountCallout.cls        # Token management
  │    ├── AwsAccountCalloutTest.cls
  │
  ├── triggers/
  │    ├── BankTrigger.trigger
  │    ├── BankTriggerHandler.cls
  │
  ├── objects/
  │    ├── Account_Access_Token__c.object   # Custom object for token storage
  │
  ├── settings/
  │    ├── AWS_Credentials__c.setting # Custom setting for environment configs
  │
  └── README.md
