---
title: "Anahtar kasası gizli Resource Manager şablonu ile | Microsoft Docs"
description: "Gizli bir anahtar Kasası'nı dağıtım sırasında parametre olarak geçirmek nasıl gösterir."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 1ca72599e67e79d42a3d430dbb13e89ea7265334
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Anahtar kasası dağıtım sırasında güvenli parametre değeri geçirmek için kullanın

Güvenli bir değerle (örneğin, parola), dağıtım sırasında parametre olarak geçirmek gerektiğinde değerini alabilir bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Anahtar kasasını ve gizli parametre dosyanıza başvurarak değerini alır. Anahtar kasası kimliğini yalnızca başvuru olduğundan değeri hiçbir zaman kullanıma Değeri için gizli anahtar kaynakları dağıttığınız her zaman el ile girmeniz gerekmez. Anahtar kasası dağıttığınız kaynak grubunu değerinden farklı bir abonelik var olabilir. Anahtar kasası başvururken abonelik kimliğini içerir

Anahtar kasası oluştururken ayarlama *enabledForTemplateDeployment* özelliğine *doğru*. Bu değeri true olarak ayarlayarak, Resource Manager şablonlarını dağıtımı sırasında erişimine.  

## <a name="deploy-a-key-vault-and-secret"></a>Bir anahtar kasası ve gizli dağıtma

Bir anahtar kasası ve gizli anahtarı oluşturmak için Azure CLI veya PowerShell kullanın. Anahtar kasası şablon dağıtım için etkinleştirildiğinde dikkat edin. 

Azure CLI için şunu kullanın:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

PowerShell için şunu kullanın:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Gizli erişimi etkinleştir

Yeni bir anahtar kasası veya varolan bir kullanıp kullanmadığınızı şablon dağıtma kullanıcı gizliliği erişebildiğinden emin olun. Gizli başvuruda bulunan bir şablonu dağıtmayı kullanıcının olmalıdır `Microsoft.KeyVault/vaults/deploy/action` anahtar kasası için izni. [Sahibi](../active-directory/role-based-access-built-in-roles.md#owner) ve [katkıda bulunan](../active-directory/role-based-access-built-in-roles.md#contributor) rolleri hem bu erişimi verin. Ayrıca bir [özel rol](../active-directory/role-based-access-control-custom-roles.md) , bu izni verir ve bu rol için kullanıcı ekleyin. Bir role kullanıcı ekleme hakkında daha fazla bilgi için bkz: [kullanıcı Azure Active Directory'de yönetici rolleri atama](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Statik Kimliğine sahip bir gizlilik başvurusu

Bir anahtar kasası gizli alan için herhangi bir şablonu gibi şablonudur. Çünkü **parametre dosyası, şablonunu değil anahtar kasasını başvuru.** Örneğin, aşağıdaki şablonu, bir yönetici parolası içeren bir SQL Veritabanını dağıtır. Parola parametresi güvenli bir dizeye ayarlayın. Ancak şablon bu değeri nereden geldiğini belirtmiyor.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

Şimdi, önceki şablonu için bir parametre dosyası oluşturun. Parametre dosyasında şablondaki parametresinin adıyla eşleşen bir parametre belirtin. Parametre değeri için gizli anahtar Kasası'nı başvuru. Gizli anahtar kasasının kaynak tanımlayıcısı ve gizli anahtar adını geçirerek başvuru. Aşağıdaki örnekte, anahtar kasası gizli anahtarı zaten mevcut olmalıdır ve kendi kaynak kimliği için statik bir değer girin

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a>Dinamik Kimliğine sahip bir gizlilik başvurusu

Önceki bölümde anahtar kasasına gizli anahtarı için bir statik kaynak kimliği geçirmek nasıl oluşturulacağını gösterir. Ancak, bazı senaryolarda, geçerli dağıtımı göre değişen bir anahtar kasası gizlilik başvuru gerekir. Bu durumda, sabit kodlu Parametreler dosyasında kaynak kimliği olamaz. Ne yazık ki, şablon ifadeleri Parametreler dosyasında izin verilmiyor çünkü parametreleri dosyasında kaynak kimliği dinamik olarak oluşturulamıyor.

Kaynak kimliği için bir anahtar kasası gizlilik dinamik olarak oluşturmak için iç içe geçmiş bir şablona gizli gereken kaynak taşımanız gerekir. Ana şablonunuzda iç içe geçmiş şablonuna ekleme ve dinamik olarak üretilen kaynak kimliği içeren bir parametre geçirin

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Anahtar kasalarını hakkında genel bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md).
* Anahtar parolaları başvuran tüm örnekler için bkz: [anahtar kasası örnekler](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

