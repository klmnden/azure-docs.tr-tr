---
title: Anahtar kasası gizli Azure Resource Manager şablonu ile | Microsoft Docs
description: Gizli bir anahtar Kasası'nı dağıtım sırasında parametre olarak geçirmek nasıl gösterir.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/11/2018
ms.author: tomfitz
ms.openlocfilehash: 6a6c1f10b5a46633785d9c26a766df9334fe1cb0
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Azure anahtar kasası dağıtım sırasında güvenli parametre değeri geçirmek için kullanın

Güvenli bir değerle (örneğin, parola), dağıtım sırasında parametre olarak geçirmek gerektiğinde değerini alabilir bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Anahtar kasasını ve gizli parametre dosyanıza başvurarak değerini alır. Anahtar kasası kimliğini yalnızca başvuru olduğundan değeri hiçbir zaman kullanıma Değeri için gizli anahtar kaynakları dağıttığınız her zaman el ile girmeniz gerekmez. Anahtar kasası dağıttığınız kaynak grubunu değerinden farklı bir abonelik var olabilir. Anahtar kasası başvururken abonelik kimliğini içerir

Anahtar kasası oluştururken ayarlama *enabledForTemplateDeployment* özelliğine *doğru*. Bu değeri true olarak ayarlayarak, Resource Manager şablonları dağıtımı sırasında izni.

## <a name="deploy-a-key-vault-and-secret"></a>Bir anahtar kasası ve gizli dağıtma

Bir anahtar kasası ve gizli anahtarı oluşturmak için Azure CLI veya PowerShell kullanın. Anahtar kasası şablon dağıtım için etkinleştirildiğinde dikkat edin. 

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

## <a name="enable-access-to-the-secret"></a>Gizli erişimi etkinleştir

Yeni bir anahtar kasası veya varolan bir kullanıp kullanmadığınızı şablon dağıtma kullanıcı gizliliği erişebildiğinden emin olun. Gizli başvuruda bulunan bir şablonu dağıtmayı kullanıcının olmalıdır `Microsoft.KeyVault/vaults/deploy/action` anahtar kasası için izni. [Sahibi](../role-based-access-control/built-in-roles.md#owner) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rolleri hem bu erişimi verin.

## <a name="reference-a-secret-with-static-id"></a>Statik Kimliğine sahip bir gizlilik başvurusu

Bir anahtar kasası gizli alan için herhangi bir şablonu gibi şablonudur. Çünkü **parametre dosyası, şablonunu değil anahtar kasasını başvuru.** Aşağıdaki resimde nasıl parametre dosyası gizli başvuruyor ve bu değeri şablona geçirir gösterir.

![Statik kimliği](./media/resource-manager-keyvault-parameter/statickeyvault.png)

Örneğin, [şablonu aşağıdaki](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) , bir yönetici parolası içeren bir SQL Veritabanını dağıtır. Parola parametresi güvenli bir dizeye ayarlayın. Ancak şablon bu değeri nereden geldiğini belirtmiyor.

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

Şimdi, önceki şablonu için bir parametre dosyası oluşturun. Parametre dosyasında şablondaki parametresinin adıyla eşleşen bir parametre belirtin. Parametre değeri için gizli anahtar Kasası'nı başvuru. Gizli anahtar kasasının kaynak tanımlayıcısı ve gizli anahtar adını geçirerek başvuru. İçinde [aşağıdaki parametre dosyasına](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.parameters.json), anahtar kasası gizli anahtarı zaten mevcut olmalıdır ve kendi kaynak kimliği için statik bir değer girin Bu dosyayı yerel olarak kopyalayın ve abonelik kimliği, kasa adı ve SQL server adı ayarlayın.

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

Geçerli sürüm dışında gizli bir sürümünü kullanmanız gerekiyorsa, kullanın `secretVersion` özelliği.

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Şimdi, şablonu dağıtmak ve parametre dosyası geçirin. Örnek şablonunu github'dan kullanabilirsiniz, ancak yerel parametre dosyası, ortamınız için ayarlanan değerleri ile kullanmanız gerekir.

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

## <a name="reference-a-secret-with-dynamic-id"></a>Dinamik Kimliğine sahip bir gizlilik başvurusu

Önceki bölümde anahtar kasasına gizli anahtarı için bir statik kaynak kimliği geçirmek nasıl oluşturulacağını gösterir. Ancak, bazı senaryolarda, geçerli dağıtımı göre değişen bir anahtar kasası gizlilik başvuru gerekir. Bu durumda sabit kodlu Parametreler dosyasında kaynak kimliği yapılamıyor. Ne yazık ki, şablon ifadeleri Parametreler dosyasında izin verilmediğinden Parametreler dosyasında kaynak kimliği dinamik olarak oluşturulamıyor.

Kaynak kimliği için bir anahtar kasası gizlilik dinamik olarak oluşturmak için bağlantılı bir şablona gizli anahtar gerekiyor kaynak taşımanız gerekir. Üst şablonunuzda bağlantılı şablonuna ekleme ve dinamik olarak üretilen kaynak kimliği içeren bir parametre geçirin Aşağıdaki resimde, bağlantılı şablonu içindeki bir parametre gizli nasıl başvuruyor gösterir.

![Dinamik kimliği](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

Bağlantılı şablonunuzu dış bir URI kullanılabilir olmalıdır. Genellikle, şablonunuza bir depolama hesabı ekleyin ve gibi URI üzerinden erişim `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json`.

[Şablonu aşağıdaki](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json) dinamik olarak anahtar kasası kimliği oluşturur ve bir parametre olarak geçirir. İçin bağlanan bir [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) github'da.

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

Yukarıdaki şablonu dağıtmak ve parametreler için değerler sağlayın. Örnek şablonunu github'dan kullanabilirsiniz, ancak ortamınız için parametre değerlerini sağlamanız gerekir.

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
* Anahtar kasalarını hakkında genel bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md).
* Anahtar parolaları başvuran tüm örnekler için bkz: [anahtar kasası örnekler](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
