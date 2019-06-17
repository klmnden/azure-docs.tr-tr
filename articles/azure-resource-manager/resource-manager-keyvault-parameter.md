---
title: Azure Resource Manager şablonu ile Key Vault gizli | Microsoft Docs
description: Gizli anahtar kasasından dağıtım sırasında parametre olarak geçirme işlemi gösterilmektedir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: tomfitz
ms.openlocfilehash: e47a087e27b6a8ade947e36ded762ce2e518ca25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65507989"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Dağıtım sırasında güvenli bir parametre geçirmek için Azure Key Vault'u kullanma

Güvenli bir değerle (parola gibi) doğrudan şablonu veya parametre dosyanıza almak yerine değerini alabilir bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md) dağıtımı sırasında. Değeri, anahtar kasasını ve gizli parametre dosyanızdaki başvurarak alın. Anahtar kasası kimliğini yalnızca başvuru değeri hiçbir zaman sunulur Anahtar kasası için dağıtıyorsanız kaynak grubundan farklı bir abonelikte bulunabilir.

## <a name="deploy-key-vaults-and-secrets"></a>Anahtar kasaları ve gizli anahtarları dağıtma

Şablon dağıtımı sırasında bir anahtar kasasına erişmek için `enabledForTemplateDeployment` için anahtar kasasındaki `true`.

Aşağıdaki Azure CLI ve Azure PowerShell örnekleri, anahtar kasası oluşturma ve bir gizli dizi eklemek gösterilmektedir.

```azurecli
az group create --name $resourceGroupName --location $location
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $keyVaultName --name "ExamplePassword" --value "hVFkk965BuUv"
```

```azurepowershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString 'hVFkk965BuUv' -AsPlainText -Force
$secret = Set-AzKeyVaultSecret -VaultName $keyVaultName -Name 'ExamplePassword' -SecretValue $secretvalue
```

Anahtar kasası sahibi olarak otomatik olarak gizli dizileri oluşturma erişebilirsiniz. Gizli dizileri ile çalışan kullanıcı anahtar kasasının sahibine değilse ile erişim:

```azurecli
az keyvault set-policy \
  --upn $userPrincipalName \
  --name $keyVaultName \
  --secret-permissions set delete get list
```

```azurepowershell
$userPrincipalName = "<Email Address of the deployment operator>"

Set-AzKeyVaultAccessPolicy `
  -VaultName $keyVaultName `
  -UserPrincipalName $userPrincipalName `
  -PermissionsToSecrets set,delete,get,list
```

Anahtar kasası oluşturma ve gizli anahtarları ekleme hakkında daha fazla bilgi için bkz:

- [Ayarlayın ve CLI kullanarak bir gizli dizi alma](../key-vault/quick-create-cli.md)
- [Ayarlayın ve Powershell kullanarak bir gizli dizi alma](../key-vault/quick-create-powershell.md)
- [Ayarlayın ve portalını kullanarak bir gizli dizi alma](../key-vault/quick-create-portal.md)
- [Ayarlayın ve .NET kullanarak bir gizli dizi alma](../key-vault/quick-create-net.md)
- [Node.js kullanarak bir gizli dizi alma ve ayarlama](../key-vault/quick-create-node.md)

## <a name="grant-access-to-the-secrets"></a>Gizli dizileri için erişim izni ver

Şablon dağıtır kullanıcının olmalıdır `Microsoft.KeyVault/vaults/deploy/action` izin kapsam için anahtar kasası ve kaynak grubu. [Sahibi](../role-based-access-control/built-in-roles.md#owner) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rollerinin her ikisi de bu erişim verin. Anahtar kasası oluşturduysanız, böylece iznine sahip sahibi olmanız.

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

    ```azurecli
    az role definition create --role-definition "<PathToRoleFile>"
    az role assignment create \
      --role "Key Vault resource manager template deployment operator" \
      --assignee $userPrincipalName \
      --resource-group $resourceGroupName
    ```

    ```azurepowershell
    New-AzRoleDefinition -InputFile "<PathToRoleFile>" 
    New-AzRoleAssignment `
      -ResourceGroupName $resourceGroupName `
      -RoleDefinitionName "Key Vault resource manager template deployment operator" `
      -SignInName $userPrincipalName
    ```

    Örnekler, kaynak grubu düzeyinde kullanıcı özel rol atayın.  

Şablonu ile bir Key Vault kullanırken bir [yönetilen uygulamayı](../managed-applications/overview.md), erişim izni vermesi gerekir **Gereci kaynak sağlayıcısı** hizmet sorumlusu. Daha fazla bilgi için [Azure yönetilen uygulamaları dağıtırken Access Key Vault gizli](../managed-applications/key-vault-access.md).

## <a name="reference-secrets-with-static-id"></a>Statik kimliğine başvuru gizli dizileri

Bu yaklaşımda, şablon parametre dosyası anahtar kasasında başvuru. Parametre dosyasını nasıl gizli dizi başvuruyor ve bu değer, şablon geçirir, aşağıdaki resimde gösterilmektedir.

![Resource Manager anahtar kasası tümleştirme statik kimliği diyagramı](./media/resource-manager-keyvault-parameter/statickeyvault.png)

[Öğretici: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme](./resource-manager-tutorial-use-key-vault.md) bu yöntemi kullanır.

Aşağıdaki şablonu yönetici parolasını içeren bir SQL server'ı dağıtır. Parola parametresi bir güvenli dizeye ayarlanır. Ancak şablon değeri nereden geldiğini belirtmiyor.

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

Şimdi, yukarıdaki şablonu için bir parametre dosyası oluşturun. Parametre dosyasında şablondaki parametresinin adıyla eşleşen bir parametre belirtin. Parametre değeri, anahtar kasasından gizli dizi başvuru. Gizli anahtar kasasının kaynak tanımlayıcısı ve gizli dizi adını geçirerek başvuru:

Şu parametre dosyası, anahtar kasası gizli dizi önceden var olmalıdır ve statik bir değer için kendi kaynak kimliği sağlayın.

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
                "id": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "ExamplePassword"
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
"secretName": "ExamplePassword",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Şablonu dağıtmak ve parametre dosyası geçirin:

Azure CLI için şunu kullanın:

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri <The Template File URI> \
    --parameters <The Parameter File>
```

PowerShell için şunu kullanın:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri <The Template File URI> `
  -TemplateParameterFile <The Parameter File>
```

## <a name="reference-secrets-with-dynamic-id"></a>Dinamik kimliğine başvuru gizli dizileri

Önceki bölümde, anahtar kasası gizli dizi için bir statik kaynak kimliği parametresinden geçirilecek gösterildi. Ancak, bazı senaryolarda, geçerli dağıtımı göre değişen bir anahtar kasası gizli dizi başvurmanız gerekir. Veya, şablona parametre değerlerini geçirmek yerine bir başvuru parametresi parametre dosyasında oluşturmak isteyebilirsiniz. Her iki durumda da bağlantılı bir şablon kullanarak dinamik olarak kaynak kimliği için bir anahtar kasası gizli dizi oluşturabilirsiniz.

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

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=$keyVaultName vaultResourceGroupName=examplegroup secretName=examplesecret
```

PowerShell için şunu kullanın:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName $keyVaultName -vaultResourceGroupName $keyVaultResourceGroupName -secretName $secretName
```

## <a name="next-steps"></a>Sonraki adımlar

- Anahtar kasası hakkında genel bilgi için bkz: [Azure anahtar kasası nedir?](../key-vault/key-vault-overview.md).
- Anahtarı parolalarını başvuru tam örnekler için bkz. [Key Vault örnek](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
