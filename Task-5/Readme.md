# Azure Key Vault Setup and Secret Management

## Commands Used

```bash
az keyvault create --name CapsKeyVaultaka --resource-group CapsResourceGroup --location centralindia

az keyvault set-policy --name CapsKeyVaultaka --upn 221106045@rcpit.ac.in --secret-permissions all --key-permissions all

az keyvault secret set --vault-name CapsKeyVaultaka --name ShieldsSecretValue --value "ThisIsShieldsSecret"


CMD Output

Microsoft Windows [Version 10.0.22000.2538]
(c) Microsoft Corporation. All rights reserved.

C:\Users\admin>az login
Select the account you want to log in with. For more information on login with Azure CLI, see https://go.microsoft.com/fwlink/?linkid=2271136
User canceled the flow. Status: Response_Status.Status_UserCanceled, Error code: 0, Tag: 593773845
Please explicitly log in with:
az login

C:\Users\admin>az login
Select the account you want to log in with. For more information on login with Azure CLI, see https://go.microsoft.com/fwlink/?linkid=2271136

Retrieving tenants and subscriptions for the selection...

[Tenant and subscription selection]

No     Subscription name    Subscription ID                       Tenant
-----  -------------------  ------------------------------------  -------------------------
[1] *  Azure for Students   979e0273-76c3-45ef-8f3a-392e1b66d40d  Shirpur Education Society

The default is marked with an *; the default tenant is 'Shirpur Education Society' and subscription is 'Azure for Students' (979e0273-76c3-45ef-8f3a-392e1b66d40d).

Select a subscription and tenant (Type a number or Enter for no changes):

Tenant: Shirpur Education Society
Subscription: Azure for Students (979e0273-76c3-45ef-8f3a-392e1b66d40d)

[Announcements]
With the new Azure CLI login experience, you can select the subscription you want to use more easily. Learn more about it and its configuration at https://go.microsoft.com/fwlink/?linkid=2271236

If you encounter any problem, please open an issue at https://aka.ms/azclibug

[Warning] The login output has been updated. Please be aware that it no longer displays the full list of available subscriptions by default.

C:\Users\admin>az provider show --namespace Microsoft.KeyVault --query "registrationState"
"Registered"

C:\Users\admin>az keyvault list --output table

C:\Users\admin>
C:\Users\admin>az provider show --namespace Microsoft.KeyVault --query "registrationState"
"Registered"

C:\Users\admin>
C:\Users\admin>az keyvault list --output table

C:\Users\admin>
C:\Users\admin>az keyvault create --name CapsKeyVaultaka --resource-group CapsResourceGroup --location centralindia
{
  "id": "/subscriptions/979e0273-76c3-45ef-8f3a-392e1b66d40d/resourceGroups/CapsResourceGroup/providers/Microsoft.KeyVault/vaults/CapsKeyVaultaka",
  "location": "centralindia",
  "name": "CapsKeyVaultaka",
  "properties": {
    "accessPolicies": [],
    "createMode": null,
    "enablePurgeProtection": null,
    "enableRbacAuthorization": true,
    "enableSoftDelete": true,
    "enabledForDeployment": false,
    "enabledForDiskEncryption": null,
    "enabledForTemplateDeployment": null,
    "hsmPoolResourceId": null,
    "networkAcls": null,
    "privateEndpointConnections": null,
    "provisioningState": "Succeeded",
    "publicNetworkAccess": "Enabled",
    "sku": {
      "family": "A",
      "name": "standard"
    },
    "softDeleteRetentionInDays": 90,
    "tenantId": "35378ff2-8912-4402-91b9-2992f3c03da3",
    "vaultUri": "https://capskeyvaultaka.vault.azure.net/"
  },
  "resourceGroup": "CapsResourceGroup",
  "systemData": {
    "createdAt": "2025-06-21T18:54:20.474000+00:00",
    "createdBy": "221106045@rcpit.ac.in",
    "createdByType": "User",
    "lastModifiedAt": "2025-06-21T18:54:20.474000+00:00",
    "lastModifiedBy": "221106045@rcpit.ac.in",
    "lastModifiedByType": "User"
  },
  "tags": {},
  "type": "Microsoft.KeyVault/vaults"
}

C:\Users\admin>az keyvault show --name CapsKeyVaultaka --query properties.enableRbacAuthorization
true

C:\Users\admin>az keyvault secret set --vault-name CapsKeyVaultaka --name ShieldsSecretValue --value "ThisIsShieldsSecret"
{
  "attributes": {
    "created": "2025-06-21T19:34:30+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoverableDays": 90,
    "recoveryLevel": "Recoverable+Purgeable",
    "updated": "2025-06-21T19:34:30+00:00"
  },
  "contentType": null,
  "id": "https://capskeyvaultaka.vault.azure.net/secrets/ShieldsSecretValue/100185aa7dc34e67a6340026d4f25140",
  "kid": null,
  "managed": null,
  "name": "ShieldsSecretValue",
  "tags": {
    "file-encoding": "utf-8"
  },
  "value": "ThisIsShieldsSecret"
}

C:\Users\admin>
C:\Users\admin>az keyvault secret show --vault-name CapsKeyVaultaka --name ShieldsSecretValue --query value -o tsv
ThisIsShieldsSecret
