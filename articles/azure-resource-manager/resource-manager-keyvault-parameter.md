---
title: Azure Resource Manager şablonu ile Key Vault gizli | Microsoft Docs
description: Gizli anahtar kasasından dağıtım sırasında parametre olarak geçirme işlemi gösterilmektedir.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: e04ee6b74be0bb7ec81166f43295a8a9377f8cf8
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54381531"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Dağıtım sırasında güvenli bir parametre geçirmek için Azure Key Vault'u kullanma

(Parola gibi) güvenli bir değerle, dağıtım sırasında parametre olarak geçirmeniz gerektiğinde, değerini almak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Değeri, anahtar kasasını ve gizli parametre dosyanızdaki başvurarak alın. Anahtar kasası kimliğini yalnızca başvuru değeri hiçbir zaman sunulur Anahtar kasası, dağıtım yaptığınız kaynak grubundan farklı bir abonelikte bulunabilir.

## <a name="deploy-a-key-vault-and-secret"></a>Bir anahtar kasası veya gizli anahtarı dağıtın

Bir anahtar kasasını ve gizli dizi oluşturmak için Azure CLI veya PowerShell kullanın. `enabledForTemplateDeployment` anahtar kasası özelliğidir. Resource Manager dağıtımından bu Key Vault içinde gizli dizileri erişmeye `enabledForTemplateDeployment` olmalıdır `true`. 

Aşağıdaki örnek Azure PowerShell ve Azure CLI betiği, Key Vault ve gizli dizi oluşturma işlemini gösterir.

Azure CLI için şunu kullanın:

```azurecli-interactive
keyVaultName='{your-unique-vault-name}'
resourceGroupName='{your-resource-group-name}'
location='centralus'
userPrincipalName='{your-email-address-associated-with-your-subscription}'

# Create a resource group
az group create --name $resourceGroupName --location $location

# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

# Create a secret with the name, vmAdminPassword
password=$(openssl rand -base64 32)
echo $password
az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
$keyVaultName = "{your-unique-vault-name}"
$resourceGroupName="{your-resource-group-name}"
$location='Central US'
$userPrincipalName='{your-email-address-associated-with-your-subscription}'

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

New-AzureRmKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list

$password = openssl rand -base64 32
echo $password
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretvalue
```

PowerShell komut dosyasını Cloud shell'e dışında çalışır, bunun yerine parola oluşturmak için aşağıdaki komutu kullanın:

```powershell
Add-Type -AssemblyName System.Web
[System.Web.Security.Membership]::GeneratePassword(16,3)
```

Resource Manager şablonu kullanmak için: Bkz: [Öğreticisi: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme](./resource-manager-tutorial-use-key-vault.md#prepare-the-key-vault).

> [!NOTE]
> Her Azure hizmetinin parola gereksinimleri farklıdır. Örneğin, Azure sanal makine gereksinimleri bölümünde bulunabilir [parola gereksinimleri bir VM oluşturulurken nelerdir?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="enable-access-to-the-secret"></a>Gizli dizi erişimi etkinleştirme

Dışında ayarlama `enabledForTemplateDeployment` için `true`, şablon dağıtımı kullanıcının olmalıdır `Microsoft.KeyVault/vaults/deploy/action` içeren kaynak grubunu ve anahtar kasası dahil olmak üzere anahtar kasası kapsam izni. [Sahibi](../role-based-access-control/built-in-roles.md#owner) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rollerinin her ikisi de bu erişim verin. Key Vault oluşturun, böylece iznine sahip sahibi olursunuz. Key Vault farklı bir abonelik altında ise, Key Vault sahibi erişim vermeniz gerekir.

Aşağıdaki yordam en az izni bir rolü nasıl oluşturulur ve kullanıcıya atanacak nasıl gösterir
1. Özel rol tanımı JSON dosyası oluşturun:

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    ": 00000000-0000-0000-0000-000000000000" şablonları dağıtmak için gereken kullanıcı abonelik kimliği ile değiştirin.

2. JSON dosyasını kullanarak yeni bir rolü oluşturun:

    ```azurepowershell
    $resourceGroupName= "<Resource Group Name>" # the resource group which contains the Key Vault
    $userPrincipalName = "<Email Address of the deployment operator>"
    New-AzureRmRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzureRmRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    `New-AzureRmRoleAssignment` Örnek kaynak grubu düzeyinde kullanıcı özel rol atayabilirsiniz.  

Şablonu ile bir Key Vault kullanırken bir [yönetilen uygulamayı](../managed-applications/overview.md), erişim izni vermesi gerekir **Gereci kaynak sağlayıcısı** hizmet sorumlusu. Daha fazla bilgi için [Azure yönetilen uygulamaları dağıtırken Access Key Vault gizli](../managed-applications/key-vault-access.md).

## <a name="reference-a-secret-with-static-id"></a>Gizli dizi ile statik kimliği başvurusu

Bir anahtar kasası gizli alan gibi herhangi bir şablon şablonudur. Çünkü **parametre dosyasında, şablon anahtar kasası başvurusu.** Parametre dosyasını nasıl gizli dizi başvuruyor ve bu değer, şablon geçirir, aşağıdaki resimde gösterilmektedir.

![Statik kimliği](./media/resource-manager-keyvault-parameter/statickeyvault.png)

Örneğin, [şablon aşağıdaki](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) yönetici parolasını içeren bir SQL veritabanı dağıtır. Parola parametresi bir güvenli dizeye ayarlanır. Ancak, bu değeri nereden geldiğini şablonu belirtmiyor.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Şimdi, yukarıdaki şablonu için bir parametre dosyası oluşturun. Parametre dosyasında şablondaki parametresinin adıyla eşleşen bir parametre belirtin. Parametre değeri, anahtar kasasından gizli dizi başvuru. Gizli anahtar kasasının kaynak tanımlayıcısı ve gizli dizi adını geçirerek başvuru. İçinde [parametre dosyası aşağıdaki](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.parameters.json), anahtar kasası gizli dizi önceden var olmalıdır ve statik bir değer için kendi kaynak kimliği sağlayın. Bu dosyayı yerel olarak kopyalayın ve abonelik kimliği, kasa adı ve SQL server adı ayarlayın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "examplesecret"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Gizli dizi geçerli sürümü dışında bir sürümünü kullanmanız gerekir, kullanın `secretVersion` özelliği.

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Şimdi, şablonu dağıtın ve parametre dosyası geçirin. Github'dan örnek şablonu kullanabilirsiniz, ancak ortamınıza ayarlanan değerleri ile yerel parametre dosyası kullanmanız gerekir.

Azure CLI için şunu kullanın:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

PowerShell için şunu kullanın:

```powershell-interactive
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Gizli dizi ile dinamik kimliği başvurusu

Önceki bölümde, anahtar kasası gizli dizi için bir statik kaynak kimliği parametresinden geçirilecek gösterildi. Ancak, bazı senaryolarda, geçerli dağıtımı göre değişen bir anahtar kasası gizli dizi başvurmanız gerekir. Veya yalnızca şablon için parametre değerlerini geçirmek yerine bir başvuru parametresi parametre dosyasında oluşturmak isteyebilirsiniz. Her iki durumda da bağlantılı bir şablon kullanarak dinamik olarak kaynak kimliği için bir anahtar kasası gizli dizi oluşturabilirsiniz.

Şablon ifadeleri Parametreler dosyasında izin verilmediğinden Parametreler dosyasında kaynak kimliği dinamik olarak oluşturulamıyor. 

Üst şablonunuza bağlı şablon ekleyin ve dinamik olarak üretilen kaynak kimliğini içeren bir parametre geçirin Aşağıdaki resimde, bağlı şablonun parametresinde gizli dizi nasıl başvuruda gösterilmektedir.

![Dinamik kimliği](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

[Şablon aşağıdaki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-key-vault-use-dynamic-id) dinamik olarak anahtar kasası kimliği oluşturur ve parametre olarak geçirir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location where the resources will be deployed."
            }
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the keyvault that contains the secret."
            }
        },
        "secretName": {
            "type": "string",
            "metadata": {
                "description": "The name of the secret."
            }
        },
        "vaultResourceGroupName": {
            "type": "string",
            "metadata": {
                "description": "The name of the resource group that contains the keyvault."
            }
        },
        "vaultSubscription": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "The name of the subscription that contains the keyvault."
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located. When the template is deployed using the accompanying scripts, a private location in the subscription will be used and this value will be automatically generated."
            },
            "defaultValue": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/"
        },
        "_artifactsLocationSasToken": {
            "type": "securestring",
            "metadata": {
                "description": "The sasToken required to access _artifactsLocation.  When the template is deployed using the accompanying scripts, a sasToken will be automatically generated."
            },
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "dynamicSecret",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "contentVersion": "1.0.0.0",
                    "uri": "[uri(parameters('_artifactsLocation'), concat('./nested/sqlserver.json', parameters('_artifactsLocationSasToken')))]"
                },
                "parameters": {
                    "location": {
                        "value": "[parameters('location')]"
                    },
                    "adminLogin": {
                        "value": "ghuser"
                    },
                    "adminPassword": {
                        "reference": {
                            "keyVault": {
                                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                            },
                            "secretName": "[parameters('secretName')]"
                        }
                    }
                }
            }
        }
    ],
    "outputs": {
        "sqlFQDN": {
            "type": "string",
            "value": "[reference('dynamicSecret').outputs.sqlFQDN.value]"
        }
    }
}
```

Yukarıdaki şablonu dağıtın ve parametreler için değerler sağlayın. Github'dan örnek şablonu kullanabilirsiniz, ancak ortamınız için parametre değerlerini sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=<your-vault> vaultResourceGroupName=examplegroup secretName=examplesecret
```

PowerShell için şunu kullanın:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName <your-vault> -vaultResourceGroupName examplegroup -secretName examplesecret
```

## <a name="next-steps"></a>Sonraki adımlar
* Anahtar kasası hakkında genel bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md).
* Anahtarı parolalarını başvuru tam örnekler için bkz. [Key Vault örnek](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).