---
title: "Azure Resource Manager kullanarak bir Redis önbelleği sağlama | Microsoft Docs"
description: "Bir Azure Redis önbelleği dağıtmak için Azure Resource Manager şablonunu kullanın."
services: app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: cce5d63e8bad2dd066cb4c28e2a8a9cb16c47953
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-redis-cache-using-a-template"></a>Şablon kullanarak Redis Önbelleği oluşturma
Bu konuda, bir Azure Redis önbelleği dağıtan bir Azure Resource Manager şablonunun nasıl oluşturulacağını öğrenin. Önbellek tanılama verilerini korumak için varolan bir depolama hesabı ile kullanılabilir. Ayrıca nasıl tanımlamak için hangi kaynağın dağıtılan ve ne zaman dağıtım yürütülen parametreler tanımlamak nasıl belirtilen öğrenin. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şu anda bir abonelik için aynı bölgede tüm önbellekler için tanılama ayarları paylaşılır. Bir önbellek bölgede güncelleştirme bölgedeki tüm diğer önbellekleri etkiler.

Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Tam şablon için bkz: [Redis önbelleği şablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Resource Manager şablonları yeni [Premium katmanı](cache-premium-tier-intro.md) kullanılabilir. 
> 
> * [Kümeleme Premium Redis önbelleği oluşturma](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Premium Redis önbelleği ile veri kalıcılığını oluşturma](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [VNet ve isteğe bağlı kümeleme Premium Redis önbelleği oluşturma](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> Son şablonları denetlemek için bkz: [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates/) arayın ve `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>Dağıtmak
Bu şablonda bir Azure Redis Tanılama verileri için mevcut bir depolama hesabını kullanan önbelleği dağıtır.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon tüm parametre değerleri içeren parametre adlı bir bölüm içerir.
Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama göre değişen değerler için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
Redis önbelleği konumu. En iyi performans için aynı konuma önbellek ile kullanılmak üzere bir uygulama olarak kullanın.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
Tanılama için kullanmak üzere var olan depolama hesabı adı. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>EnableNonSslPort
SSL olmayan bağlantı noktaları erişimine izin verilip verilmeyeceğini gösteren bir Boole değeri.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Tanılama etkin olup olmadığını belirten bir değer. Kullanım açık veya kapalı.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
### <a name="redis-cache"></a>Redis Önbelleği
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
          "apiVersion": "2015-07-01",
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


