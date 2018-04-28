---
title: Bir Azure Resource Manager şablonu ile Azure Machine Learning deneme oluşturma | Microsoft Docs
description: Bu makalede Azure Resource Manager şablonu kullanarak bir Azure Machine Learning deneme hesabı oluşturmak için bir örnek sağlar.
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/14/2017
ms.openlocfilehash: 6f3e08cf36a27d8f42291d13e82458e3fcebc03f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="configure-the-azure-machine-learning-experimentation-service"></a>Azure Machine Learning deneme hizmetini yapılandırma

## <a name="overview"></a>Genel Bakış
Azure Machine Learning Deneme hizmet hesabı, çalışma ve proje Azure kaynaklardır. Bu nedenle, bunlar kaynakları Yöneticisi şablonları kullanılarak dağıtılabilir. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetmeyle ilgili kavramları anlamak için bkz. [Azure Resource Manager’a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="deploy-a-template"></a>Şablon dağıtma
Bir şablon dağıtımı yalnızca birkaç adım, Azure komut satırı arabirimini veya Azure portalında gerektirir.

### <a name="deploy-a-template-from-the-command-line"></a>Komut satırından şablon dağıtma
Tek bir komut, komut satırı arabirimi kullanarak, varolan bir kaynak grubuna şablon dağıtabilirsiniz.
Bir şablon oluşturma hakkında daha fazla bilgi için bkz: aşağıdakileri.

```sh
# Login and validate your are in the right subscription context
az login

# Create a new resource group (you can use an existing one)
az group create --name <resource group name> --location <supported Azure region>
az group deployment create -n testdeploy -g <resource group name> --template-file <template-file.json> --parameters <parameters.json>
```

### <a name="deploy-a-template-from-the-azure-portal"></a>Azure portalından bir şablon dağıtma
İsterseniz, Azure portal oluşturmak ve bir şablonu dağıtmak için kullanabilirsiniz. Aşağıdaki gibi yapın:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Seçin **tüm hizmetleri** ve arama "şablonlarının."
3. Seçin **şablonları**.
4. Tıklayın **+ Ekle** ve şablon bilgilerinizi kopyalayın. 
5. #4. adımda oluşturduğunuz şablonu seçin ve'ı tıklatın **dağıtma**.


## <a name="create-a-template-from-an-existing-azure-resource-in-the-azure-portal"></a>Azure Portalı'ndaki mevcut bir Azure kaynağı bir şablon oluşturun
İçinde kullanılabilir hesap bir Azure Machine deneme zaten yüklü olup olmadığını [Azure portal](https://portal.azure.com), bu kaynaktan bir şablon oluşturabilirsiniz. 

1. Bir Azure deneme hesabına gidin [Azure portal](https://portal.azure.com).
2. Altında **ayarları**, tıklayın **Otomasyon betiğini**.
3. Şablon indirin. 

Alternatif olarak, şablon dosyalarını el ile düzenleyebilirsiniz. Aşağıdaki dosyaları bir şablon ve parametreleri bir örnek için bkz. 

### <a name="template-file-example"></a>Şablon dosyası örneği
İçerik ile "şablonu-file.json" adlı bir dosya oluşturun. 

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
İçerik aşağıda bir dosya oluşturun ve < parameters.json > kaydedin. 

Değiştirebileceğiniz üç değerden vardır. 
* AccountName: Deneme hesabının adı.
* Konumu: Biri desteklenen Azure bölgeleri.
* Depolama hesabı SKU: Azure ML standart depolama, premium değil yalnızca destekler. Depolama hakkında daha fazla bilgi için bkz: [depolama giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction). 

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
* [Oluşturun ve Azure Machine Learning yükleyin](../service/quickstart-installation.md)
