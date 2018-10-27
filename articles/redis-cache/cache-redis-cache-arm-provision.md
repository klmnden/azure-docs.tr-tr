---
title: Azure Resource Manager kullanarak Redis önbelleği sağlama | Microsoft Docs
description: Azure Redis Cache dağıtmak için Azure Resource Manager şablonu kullanın.
services: app-service
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: c4c95218ca88ce1fe832fda4fabaf957fdd69dc1
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140375"
---
# <a name="create-a-redis-cache-using-a-template"></a>Şablon kullanarak Redis Önbelleği oluşturma
Bu konu başlığında, bir Azure Redis Cache'e dağıtan bir Azure Resource Manager şablonunun nasıl oluşturulacağını öğrenin. Önbellek ile mevcut bir depolama hesabı Tanılama verileri tutmak için kullanılabilir. Ayrıca tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen nasıl öğrenin. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şu anda, tanılama ayarları için bir abonelik için aynı bölgede tüm önbellekleri paylaşılır. Bir önbellek bölgede güncelleştirme bölgesindeki tüm önbellekleri etkiler.

Şablonları oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Tam şablon için bkz: [Redis Cache şablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Yeni Resource Manager şablonları [Premium katmanı](cache-premium-tier-intro.md) kullanılabilir. 
> 
> * [Kümeleme ile Premium Redis önbelleği oluşturma](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Veri kalıcılığı Premium Redis önbelleği oluşturma](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [VNet ve isteğe bağlı Kümeleme ile Premium Redis önbelleği oluşturma](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> En yeni şablonları denetlemek için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/) araması `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>Ne dağıtacaksınız
Bu şablonda bir Azure depolama hesabınız için Tanılama verileri kullanan cache'i dağıtır.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon parametreleri olarak adlandırılan bir tüm parametre değerlerini içeren bölüm içerir.
Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama göre değişen değerler için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
Redis önbelleği konumu. En iyi performans için önbellek ile kullanılmak üzere bir uygulama olarak aynı konumu kullanın.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
Tanılama için kullanılacak mevcut depolama hesabı adı. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
SSL olmayan bağlantı noktaları erişime izin verilip verilmeyeceğini gösteren bir Boole değeri.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Tanılama etkinleştirilip etkinleştirilmeyeceğini gösteren bir değer. Kullanım açık veya kapalı.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
### <a name="redis-cache"></a>Redis Cache
Azure Redis önbelleği oluşturur.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2017-05-01-preview",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


