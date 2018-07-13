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
ms.date: 07/09/2018
ms.author: tomfitz
ms.openlocfilehash: 3a29319a0d478537dfc4905ee77865b8fea64587
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38598416"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Dağıtım sırasında güvenli bir parametre geçirmek için Azure Key Vault'u kullanma

(Parola gibi) güvenli bir değerle, dağıtım sırasında parametre olarak geçirmeniz gerektiğinde, değerini almak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Değeri, anahtar kasasını ve gizli parametre dosyanızdaki başvurarak alın. Anahtar kasası kimliğini yalnızca başvuru değeri hiçbir zaman sunulur Anahtar kasası, dağıtım yaptığınız kaynak grubundan farklı bir abonelikte bulunabilir.

## <a name="enable-access-to-the-secret"></a>Gizli dizi erişimi etkinleştirme

Şablon dağıtımı sırasında bir anahtar kasasına erişmek için bulunması gereken iki önemli koşullar vardır:

1. Anahtar kasası özelliği `enabledForTemplateDeployment` olmalıdır `true`.
2. Şablonu dağıtarak kullanıcı, gizli dizi erişiminiz olması gerekir. Kullanıcının olmalıdır `Microsoft.KeyVault/vaults/deploy/action` anahtar kasası için izni. [Sahibi](../role-based-access-control/built-in-roles.md#owner) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rollerinin her ikisi de bu erişim verin.

Şablonu ile bir Key Vault kullanırken bir [yönetilen uygulamayı](../managed-applications/overview.md), erişim izni vermesi gerekir **Gereci kaynak sağlayıcısı** hizmet sorumlusu. Daha fazla bilgi için [Azure yönetilen uygulamaları dağıtırken Access Key Vault gizli](../managed-applications/key-vault-access.md).


## <a name="deploy-a-key-vault-and-secret"></a>Bir anahtar kasası veya gizli anahtarı dağıtın

Bir anahtar kasasını ve gizli dizi oluşturmak için Azure CLI veya PowerShell kullanın. Anahtar kasası şablon dağıtımı için etkinleştirildiğini dikkat edin. 

Azure CLI için şunu kullanın:

```azurecli-interactive
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create \
  --name $vaultname \
  --resource-group examplegroup \
  --location 'South Central US' \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

PowerShell için şunu kullanın:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault `
  -VaultName $vaultname `
  -ResourceGroupName examplegroup `
  -Location "South Central US" `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

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
az group create --name datagroup --location "South Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

PowerShell için şunu kullanın:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "South Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Gizli dizi ile dinamik kimliği başvurusu

Önceki bölümde, anahtar kasası gizli dizi için bir statik kaynak kimliği geçirilecek gösterildi. Ancak, bazı senaryolarda, geçerli dağıtımı göre değişen bir anahtar kasası gizli dizi başvurmanız gerekir. Bu durumda, sabit kodlu Parametreler dosyasında kaynak kimliği olamaz. Ne yazık ki şablon ifadeleri Parametreler dosyasında izin verilmediğinden Parametreler dosyasında kaynak kimliği dinamik olarak oluşturulamıyor.

Dinamik olarak kaynak kimliği için bir anahtar kasası gizli dizi oluşturmak için bağlı bir şablona gizli dizi gereken kaynak taşımanız gerekir. Üst şablonunuza bağlı şablon ekleyin ve dinamik olarak üretilen kaynak kimliğini içeren bir parametre geçirin Aşağıdaki resimde, bağlı şablonun parametresinde gizli dizi nasıl başvuruda gösterilmektedir.

![Dinamik kimliği](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

Bağlantılı şablonunuzu dış bir URI kullanılabilir olması gerekir. Genellikle, şablonunuzu bir depolama hesabına ekleyin ve gibi bir URI aracılığıyla erişim `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json`.

[Şablon aşağıdaki](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json) dinamik olarak anahtar kasası kimliği oluşturur ve parametre olarak geçirir. Bağlantı bir [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) github'da.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "vaultResourceGroup": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      },
      "adminLogin": {
        "type": "string"
      },
      "sqlServerName": {
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
          "uri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(subscription().subscriptionId,  parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          },
          "adminLogin": { "value": "[parameters('adminLogin')]" },
          "sqlServerName": {"value": "[parameters('sqlServerName')]"}
        }
      }
    }],
    "outputs": {}
}
```

Yukarıdaki şablonu dağıtın ve parametreler için değerler sağlayın. Github'dan örnek şablonu kullanabilirsiniz, ancak ortamınız için parametre değerlerini sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az group create --name datagroup --location "South Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json \
    --parameters vaultName=<your-vault> vaultResourceGroup=examplegroup secretName=examplesecret adminLogin=exampleadmin sqlServerName=<server-name>
```

PowerShell için şunu kullanın:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "South Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json `
  -vaultName <your-vault> -vaultResourceGroup examplegroup -secretName examplesecret -adminLogin exampleadmin -sqlServerName <server-name>
```

## <a name="next-steps"></a>Sonraki adımlar
* Anahtar kasası hakkında genel bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md).
* Anahtarı parolalarını başvuru tam örnekler için bkz. [Key Vault örnek](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
