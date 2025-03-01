# Azure Instances not running FlexNet Inventory Agent

## What it does

This policy uses a Flexnet Manger Cloud/On-premise instance and checks all instances running in Azure to determine if the FlexNet Inventory Agent is running on the instance and reports on any that are missing the agent.
The policy is a recommendation only policy, no action is taken during the Policy Escalation.

## Functional Details

The policy leverages the cloud API to get all current instances and the FlexNet Manager report (Custom view) API to get all azure cloud instances with agent. It cross-checks the two lists to determine if any instances are running on the cloud that aren't known to FlexNet Manager.  The policy matches the ComputerName from FlexNet Manager System and the VirtualMachine.name from Azure.

Current limitations:

- Output is limited to max 100000 rows.

## Input Parameters

This policy has the following input parameters required when launching the policy.

- *Email addresses to notify* - Email addresses of the recipients you wish to notify when new incidents are created
- *Azure Endpoint* - Azure Endpoint to access resources
- *Subscription Whitelist* - Whitelisted Subscriptions, if empty, all subscriptions will be checked
- *Exclusion Tag Key* - Azure-native Virtual machines tag to ignore VM's which has FNMS inventory agent running. Only supply the tag key. The policy assumes that the tag value is irrelevant.
- *FNMS Report URL* - Full FlexNet URL (e.g. <https://demo.flexnetmanager.com/Suite> or WStunnel tunnel URL <https://wstunnel1-1.rightscale.com/_token/<token>/>)
- *FNMS Report ID* - FlexNet manager Custom View ID.

## Policy Actions

- Send an email report

## Prerequisites

For on premise If FlexNet Manager Suite is not accessible from the Internet, you will need to setup a wstunnel to provide a secure connection into the FlexNet manager system. For more details on wstunnel please refer to this: [https://github.com/rightscale/wstunnel](https://github.com/rightscale/wstunnel)

This policy uses [credentials](https://docs.flexera.com/flexera/EN/Automation/ManagingCredentialsExternal.htm) for connecting to the cloud -- in order to apply this policy you must have a credential registered in the system that is compatible with this policy. If there are no credentials listed when you apply the policy, please contact your cloud admin and ask them to register a credential that is compatible with this policy. The information below should be consulted when creating the credential.

### Credential configuration

For administrators [creating and managing credentials](https://docs.flexera.com/flexera/EN/Automation/ManagingCredentialsExternal.htm) to use with this policy, the following information is needed:

Provider tag value to match this policy: `azure_rm` , `flexera_fnms`

- Credential type for fnms:

  - *API Key* - For FNMS Cloud  -  (*Location*:header, *Type*: Bearer)

  or
  - *NTLM* - For On Premise FNMS

Required permissions in the provider azure_rm:

- `Reader`

## Installation

### How to setup FlexNet Manager Custom View for this policy

a. Cloud

1. Create a custom view in FlexNet manager that could look like this: ![Alt text][FNMSReport]

Click on Preview and filter.
Select `Microsoft Azure` under `Inventory device` > `Hosted in` ![Alt text][FilterFNMSReport]

Once saved, note the report number in the URL field : ![Alt text][ReportNumber] you need it when activating the Policy for 'FNMS Report ID'.

1. Retrieve the API Token in FlexNet Manager System:
    1. On the Account page - Select Create Account -> Service Account and fill in the form ![Alt text][CreateServeceAccount]
    1. IMPORTANT: When you hit save you will see a API Token.. This is the only time you will see it so you need to save it at this point ![Alt text][APIToken]
    1. Add the new account to the Role ___Webservice___ ![Alt text][WebServiceRole]

__NOTE__: You can use a normal interactive user for the API credentials, but it is recommended to add a special service user for the API connection.

b. On Premise

1. Create a custom view in FlexNet manager that could look like this: ![Alt text][FNMSReport]

Click on Preview and filter.
Select `Microsoft Azure` under `Inventory device` > `Hosted in` ![Alt text][FilterFNMSReport]

Once saved, note the report number in the URL field : ![Alt text][ReportNumber] you need it when activating the Policy for 'FNMS Report ID'.

1. Set Up user for FlexNet manager on-premise:
    1. In your user management add the new user and assign it a password.
    1. On the Account page - Select Create Account -> Service Account ![Alt text][CreateServeceAccount]
    1. in the Account field; select the newly created account and fill in the form.
    1. Add the new account to the Role ___Webservice___ ![Alt text][WebServiceRole]

__NOTE__: You can use a normal interactive user for the API credentials, but it is recommended to add a special service user for the API connection.

## Supported Clouds

- Azure

## Cost

This Policy Template does not incur any cloud costs.

<!-- Image referances -->
[APIToken]: images/APIToken.png "APIToken"
[CreateServeceAccount]: images/CreateServeceAccount.png "Create Service Account"
[FNMSReport]: images/FNMS_cv_Report.png "FNMS Cloud Instance Report"
[FilterFNMSReport]: images/Filter_FNMS_Report.PNG "FNMS Microsoft Azure Instance Report"
[ReportNumber]: images/ReportNumber.png "ReportNumber"
[WebServiceRole]: images/WebServiceRole.png "WebServiceRole"
[CMPToken]: images/CMP_NewToken.png "CMP Token"
