---
title: Bir Azure Resource Manager şablonu ile Azure Machine Learning denemesi oluşturma | Microsoft Docs
description: Bu makalede bir Azure Resource Manager şablonu kullanarak bir Azure Machine Learning denemesi hesabı oluşturmak için bir örnek sağlar.
services: machine-learning
author: hning86
ms.author: haining
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 24ed164f4a1dfdb9a3913efa78fe58fab2b53696
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991972"
---
# <a name="configure-the-azure-machine-learning-experimentation-service"></a>Azure Machine Learning deneme hizmeti yapılandırın

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

## <a name="overview"></a>Genel Bakış
Azure Machine Learning deneme hizmeti hesabı, çalışma alanı ve proje Azure kaynaklarıdır. Bu nedenle, bunlar Resource Manager şablonları kullanılarak dağıtılabilir. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetmeyle ilgili kavramları anlamak için bkz. [Azure Resource Manager’a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="deploy-a-template"></a>Şablon dağıtma
Şablon dağıtımı, yalnızca birkaç adım, Azure komut satırı arabirimi veya Azure portalında gerektirir.

### <a name="deploy-a-template-from-the-command-line"></a>Şablon komut satırından dağıtma
Tek bir komut, komut satırı arabirimini kullanarak, mevcut bir kaynak grubu için bir şablonu dağıtabilirsiniz.
Bir şablon oluşturma hakkında daha fazla bilgi için aşağıdakilere bakın.

```sh
# Login and validate your are in the right subscription context
az login

# Create a new resource group (you can use an existing one)
az group create --name <resource group name> --location <supported Azure region>
az group deployment create -n testdeploy -g <resource group name> --template-file <template-file.json> --parameters <parameters.json>
```

### <a name="deploy-a-template-from-the-azure-portal"></a>Azure portalından bir şablon dağıtma
Tercih ederseniz, bir şablon oluşturma ve dağıtma için Azure portalını da kullanabilirsiniz. Şu şekilde yapabilirsiniz:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Seçin **tüm hizmetleri** ve "Şablon" arama
3. Seçin **şablonları**.
4. Tıklayarak **+ Ekle** ve şablon bilgilerinizi kopyalayın. 
5. #4. adımda oluşturduğunuz şablonu seçin ve tıklayın **Dağıt**.


## <a name="create-a-template-from-an-existing-azure-resource-in-the-azure-portal"></a>Bir şablonu Azure portalında mevcut bir Azure kaynağı oluşturma
İçinde kullanılabilir hesap bir Azure Machine denemesi zaten yüklü olup olmadığını [Azure portalında](https://portal.azure.com), bu kaynağı, bir şablon oluşturabilirsiniz. 

1. Bir Azure deneme hesabına gidin [Azure portalında](https://portal.azure.com).
2. Altında **ayarları**, tıklayarak **Otomasyon betiği**.
3. Şablonu indirebilir. 

Alternatif olarak, şablon dosyalarını el ile düzenleyebilirsiniz. Aşağıdaki dosyaları bir şablon ve parametreleri örneği için bkz. 

### <a name="template-file-example"></a>Şablon dosyası örneği
Altındaki içerik "şablonu file.json" adlı bir dosya oluşturun. 

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "Name of the machine learning experimentation team account"
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of the machine learning experimentation account and other dependent resources."
            }
        },
        "storageAccountSku": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "metadata": {
                "description": "Sku of the storage account"
            }
        }
    },
    "variables": {
        "mlexpVersion": "2017-05-01-preview",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
    },
    "resources": [
        {
            "name": "[parameters('accountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2016-12-01",
            "tags": {
                "mlteamAccount": "[parameters('accountName')]"
            },
            "sku": {
                "name": "[parameters('storageAccountSku')]"
            },
            "kind": "Storage",
            "properties": {
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            }
        },
        {
            "apiVersion": "[variables('mlexpVersion')]",
            "type": "Microsoft.MachineLearningExperimentation/accounts",
            "name": "[parameters('accountName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
            ],
            "properties": {
                "storageAccount": {
                    "storageAccountId": "[variables('stgResourceId')]",
                    "accessKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName')), '2016-12-01').keys[0].value]"
                }
            }
        }
    ]
}
```

### <a name="parameters"></a>Parametreler 
Aşağıdaki içeriğe sahip bir dosya oluşturun ve < parameters.json > kaydedin. 

Değişiklik yapabileceğiniz üç değer vardır. 
* AccountName: Deneme hesabı adı.
* Konum: Desteklenen Azure bölgelerinden birini.
* Depolama hesabı SKU: Azure ML standart depolama, premium değil yalnızca destekler. Depolama hakkında daha fazla bilgi için bkz. [depolama giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction). 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "accountName": {
         "value": "MyExperimentationAccount"
     },
     "location": {
         "value": "eastus2"
     },
     "storageAccountSku": {
         "value": "Standard_LRS"
     }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* [Oluşturma ve Azure Machine Learning'i yükleme](quickstart-installation.md)
