---
title: Manage user assigned managed identities - Azure AD
description: Create user assigned managed identities
services: active-directory
author: barclayn
manager: daveba
editor: 
ms.service: active-directory
ms.subservice: msi
ms.devlang: 
ms.topic: how-to
ms.workload: identity
ms.date: 06/08/2021
ms.author: barclayn
zone_pivot_groups: identity-mi-methods
---

# Manage user-assigned managed identities


Managed identities for Azure resources eliminate the need to manage credentials in code. They allow you to get an Azure active directory token your applications can use when accessing resources that support Azure Active Directory authentication. Azure manages the identity so you don't have to. There are two types of managed identities – system-assigned and user-assigned. The main difference between the two types is that system assigned managed identities have their lifecycle linked to the resource where they are used. User assigned managed identities may be used on multiple resources. You can learn more about managed identities in the managed identities [overview](overview.md). 

::: zone pivot="identity-mi-methods-azp"

In this article, you learn how to create, list, delete, or assign a role to a user-assigned managed identity using the Azure portal

## Prerequisites

- If you're unfamiliar with managed identities for Azure resources, check out the [overview section](overview.md). **Be sure to review the [difference between a system-assigned and user-assigned managed identity](overview.md#managed-identity-types)**.
- If you don't already have an Azure account, [sign up for a free account](https://azure.microsoft.com/free/) before continuing.

## Create a user-assigned managed identity

To create a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

1. Sign in to the [Azure portal](https://portal.azure.com) using an account associated with the Azure subscription to create the user-assigned managed identity.
2. In the search box, type *Managed Identities*, and under **Services**, click **Managed Identities**.
3. Click **Add** and enter values in the following fields under **Create user assigned managed** identity pane:
    - **Subscription**: Choose the subscription to create the user-assigned managed identity under.
    - **Resource group**: Choose a resource group to create the user-assigned managed identity in or click **Create new** to create a new resource group.
    - **Region**: Choose a region to deploy the user-assigned managed identity, for example **West US**.
    - **Name**: This is the name for your user-assigned managed identity, for example UAI1.
    ![Create a user-assigned managed identity](media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)
4. Click **Review + create** to review the changes.
5. Click **Create**.

## List user-assigned managed identities

To list/read a user-assigned managed identity, your account needs the [Managed Identity Operator](../../role-based-access-control/built-in-roles.md#managed-identity-operator) or [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

1. Sign in to the [Azure portal](https://portal.azure.com) using an account associated with the Azure subscription to list the user-assigned managed identities.
2. In the search box, type *Managed Identities*, and under Services, click **Managed Identities**.
3. A list of the user-assigned managed identities for your subscription is returned.  To see the details of a user-assigned managed identity click its name.

![List user-assigned managed identity](media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## Delete a user-assigned managed identity

To delete a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

Deleting a user assigned identity does not remove it from the VM or resource it was assigned to.  To remove the user assigned identity from a VM see, [Remove a user-assigned managed identity from a VM](qs-configure-portal-windows-vm.md#remove-a-user-assigned-managed-identity-from-a-vm).

1. Sign in to the [Azure portal](https://portal.azure.com) using an account associated with the Azure subscription to delete a user-assigned managed identity.
2. Select the user-assigned managed identity and click **Delete**.
3. Under the confirmation box choose, **Yes**.

![Delete user-assigned managed identity](media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## Assign a role to a user-assigned managed identity 

To assign a role to a user-assigned managed identity, your account needs the [User Access Administrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) role assignment.

1. Sign in to the [Azure portal](https://portal.azure.com) using an account associated with the Azure subscription to list the user-assigned managed identities.
2. In the search box, type *Managed Identities*, and under Services, click **Managed Identities**.
3. A list of the user-assigned managed identities for your subscription is returned.  Select the user-assigned managed identity that you want to assign a role.
4. Select **Access control (IAM)**, and then select **Add role assignment**.

   ![User-assigned managed identity start](media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. In the Add role assignment blade, configure the following values, and then click **Save**:
   - **Role** - the role to assign
   - **Assign access to**  - the resource to assign the user-assigned managed identity
   - **Select** - the member to assign access
   
   ![User-assigned managed identity IAM](media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)



::: zone-end

::: zone pivot="identity-mi-methods-azcli"

In this article, you learn how to create, list, delete, or assign a role to a user-assigned managed identity using the Azure CLI.

## Prerequisites

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../../includes/azure-cli-prepare-your-environment-no-header.md)]

> [!IMPORTANT]	
> To modify user permissions when using an app service principal using CLI you must provide the service principal additional permissions in Azure AD Graph API as portions of CLI perform GET requests against the Graph API. Otherwise, you may end up receiving a 'Insufficient privileges to complete the operation' message. To do this you will need to go into the App registration in Azure Active Directory, select your app, click on API permissions, scroll down and select Azure Active Directory Graph. From there select Application permissions, and then add the appropriate permissions. 

## Create a user-assigned managed identity 

To create a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

Use the [az identity create](/cli/azure/identity#az_identity_create) command to create a user-assigned managed identity. The `-g` parameter specifies the resource group where to create the user-assigned managed identity, and the `-n` parameter specifies its name. Replace the `<RESOURCE GROUP>` and `<USER ASSIGNED IDENTITY NAME>` parameter values with your own values:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## List user-assigned managed identities

To list/read a user-assigned managed identity, your account needs the [Managed Identity Operator](../../role-based-access-control/built-in-roles.md#managed-identity-operator) or [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

To list user-assigned managed identities, use the [az identity list](/cli/azure/identity#az_identity_list) command. Replace the `<RESOURCE GROUP>` with your own value:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```

In the json response, user-assigned managed identities have `"Microsoft.ManagedIdentity/userAssignedIdentities"` value returned for key, `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## Delete a user-assigned managed identity

To delete a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

To delete a user-assigned managed identity, use the [az identity delete](/cli/azure/identity#az_identity_delete) command.  The -n parameter specifies its name and the -g parameter specifies the resource group where the user-assigned managed identity was created. Replace the `<USER ASSIGNED IDENTITY NAME>` and `<RESOURCE GROUP>` parameters values with your own values:

```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Deleting a user-assigned managed identity will not remove the reference, from any resource it was assigned to. Please remove those from VM/VMSS using the `az vm/vmss identity remove` command

## Next steps

For a full list of Azure CLI identity commands, see [az identity](/cli/azure/identity).

For information on how to assign a user-assigned managed identity to an Azure VM see, [Configure managed identities for Azure resources on an Azure VM using Azure CLI](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)


::: zone-end

::: zone pivot="identity-mi-methods-powershell"

In this article, you learn how to create, list, and delete a user-assigned managed identity using PowerShell.

## Prerequisites

- If you're unfamiliar with managed identities for Azure resources, check out the [overview section](overview.md). **Be sure to review the [difference between a system-assigned and user-assigned managed identity](overview.md#managed-identity-types)**.
- If you don't already have an Azure account, [sign up for a free account](https://azure.microsoft.com/free/) before continuing.
- To run the example scripts, you have two options:
    - Use the [Azure Cloud Shell](../../cloud-shell/overview.md), which you can open using the **Try It** button on the top right corner of code blocks.
    - Run scripts locally with Azure PowerShell, as described in the next section.

### Configure Azure PowerShell locally

To use Azure PowerShell locally for this article (rather than using Cloud Shell), complete the following steps:

1. Install [the latest version of Azure PowerShell](/powershell/azure/install-az-ps) if you haven't already.

1. Sign in to Azure:

    ```azurepowershell
    Connect-AzAccount
    ```

1. Install the [latest version of PowerShellGet](/powershell/scripting/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).

    ```azurepowershell
    Install-Module -Name PowerShellGet -AllowPrerelease
    ```

    You may need to `Exit` out of the current PowerShell session after you run this command for the next step.

1. Install the prerelease version of the `Az.ManagedServiceIdentity` module to perform the user-assigned managed identity operations in this article:

    ```azurepowershell
    Install-Module -Name Az.ManagedServiceIdentity -AllowPrerelease
    ```

## Create a user-assigned managed identity

To create a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

To create a user-assigned managed identity, use the `New-AzUserAssignedIdentity` command. The `ResourceGroupName` parameter specifies the resource group where to create the user-assigned managed identity, and the `-Name` parameter specifies its name. Replace the `<RESOURCE GROUP>` and `<USER ASSIGNED IDENTITY NAME>` parameter values with your own values:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```

## List user-assigned managed identities

To list/read a user-assigned managed identity, your account needs the [Managed Identity Operator](../../role-based-access-control/built-in-roles.md#managed-identity-operator) or [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

To list user-assigned managed identities, use the [Get-AzUserAssigned] command.  The `-ResourceGroupName` parameter specifies the resource group where the user-assigned managed identity was created. Replace the `<RESOURCE GROUP>` with your own value:

```azurepowershell-interactive
Get-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```

In the response, user-assigned managed identities have `"Microsoft.ManagedIdentity/userAssignedIdentities"` value returned for key, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## Delete a user-assigned managed identity

To delete a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

To delete a user-assigned managed identity, use the `Remove-AzUserAssignedIdentity` command.  The `-ResourceGroupName` parameter specifies the resource group where the user-assigned identity was created and the `-Name` parameter specifies its name. Replace the `<RESOURCE GROUP>` and the `<USER ASSIGNED IDENTITY NAME>` parameters values with your own values:

```azurepowershell-interactive
Remove-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```

> [!NOTE]
> Deleting a user-assigned managed identity will not remove the reference, from any resource it was assigned to. Identity assignments need to be removed separately.

## Next steps

For a full list and more details of the Azure PowerShell managed identities for Azure resources commands, see [Az.ManagedServiceIdentity](/powershell/module/az.managedserviceidentity#managed_service_identity).


::: zone-end


::: zone pivot="identity-mi-methods-arm"

In this article, you create a user-assigned managed identity using an Azure Resource Manager.

It is not possible to list and delete a user-assigned managed identity using an Azure Resource Manager template.  See the following articles to create and list a user-assigned managed identity:

- [List user-assigned managed identity](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Delete user-assigned managed identity](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)

## Prerequisites

- If you are unfamiliar with managed identities for Azure resources, check out the [overview section](overview.md). **Be sure to review the [difference between a system-assigned and user-assigned managed identity](overview.md#managed-identity-types)**.
- If you don't already have an Azure account, [sign up for a free account](https://azure.microsoft.com/free/) before continuing.

## Template creation and editing

As with the Azure portal and scripting, Azure Resource Manager templates provide the ability to deploy new or modified resources defined by an Azure resource group. Several options are available for template editing and deployment, both local and portal-based, including:

- Using a [custom template from the Azure Marketplace](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template), which allows you to create a template from scratch, or base it on an existing common or [quickstart template](https://azure.microsoft.com/documentation/templates/).
- Deriving from an existing resource group, by exporting a template from either [the original deployment](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates), or from the [current state of the deployment](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Using a local [JSON editor (such as VS Code)](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md), and then uploading and deploying by using PowerShell or CLI.
- Using the Visual Studio [Azure Resource Group project](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md) to both create and deploy a template. 

## Create a user-assigned managed identity 

To create a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

To create a user-assigned managed identity, use the following template. Replace the `<USER ASSIGNED IDENTITY NAME>` value with your own values:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('resourceName')]",
      "apiVersion": "2018-11-30",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('resourceName')]"
      }
  }
}
```
## Next steps

For information on how to assign a user-assigned managed identity to an Azure VM using an Azure Resource Manager template see, [Configure managed identities for Azure resources on an Azure VM using a templates](qs-configure-template-windows-vm.md).




::: zone-end


::: zone pivot="identity-mi-methods-rest"

In this article, you learn how to create, list, and delete a user-assigned managed identity using CURL to make REST API calls.

## Prerequisites

- If you're unfamiliar with managed identities for Azure resources, check out the [overview section](overview.md). **Be sure to review the [difference between a system-assigned and user-assigned managed identity](overview.md#managed-identity-types)**.
- If you don't already have an Azure account, [sign up for a free account](https://azure.microsoft.com/free/) before continuing.
- You can run all the commands in this article either in the cloud or locally:
    - To run in the cloud, use the [Azure Cloud Shell](../../cloud-shell/overview.md).
    - To run locally, install [curl](https://curl.haxx.se/download.html) and the [Azure CLI](/cli/azure/install-azure-cli).

## Obtain a bearer access token

1. If running locally, sign into Azure through the Azure CLI:

    ```
    az login
    ```

1. Obtain an access token using [az account get-access-token](/cli/azure/account#az_account_get_access_token)

    ```azurecli-interactive
    az account get-access-token
    ```

## Create a user-assigned managed identity 

To create a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```

**Request headers**

|Request header  |Description  |
|---------|---------|
|*Content-Type*     | Required. Set to `application/json`.        |
|*Authorization*     | Required. Set to a valid `Bearer` access token.        |

**Request body**

|Name  |Description  |
|---------|---------|
|location     | Required. Resource location.        |

## List user-assigned managed identities

To list/read a user-assigned managed identity, your account needs the [Managed Identity Operator](../../role-based-access-control/built-in-roles.md#managed-identity-operator) or [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview HTTP/1.1
```

|Request header  |Description  |
|---------|---------|
|*Content-Type*     | Required. Set to `application/json`.        |
|*Authorization*     | Required. Set to a valid `Bearer` access token.        |

## Delete a user-assigned managed identity

To delete a user-assigned managed identity, your account needs the [Managed Identity Contributor](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) role assignment.

> [!NOTE]
> Deleting a user-assigned managed identity will not remove the reference from any resource it was assigned to. To remove a user-assigned managed identity from a VM using CURL see [Remove a user-assigned identity from an Azure VM](qs-configure-rest-vm.md#remove-a-user-assigned-managed-identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
DELETE https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```
|Request header  |Description  |
|---------|---------|
|*Content-Type*     | Required. Set to `application/json`.        |
|*Authorization*     | Required. Set to a valid `Bearer` access token.        |

## Next steps

For information on how to assign a user-assigned managed identity to an Azure VM/VMSS using CURL see, [Configure managed identities for Azure resources on an Azure VM using REST API calls](qs-configure-rest-vm.md#user-assigned-managed-identity) and [Configure managed identities for Azure resources on a virtual machine scale set using REST API calls](qs-configure-rest-vmss.md#user-assigned-managed-identity).

::: zone-end


