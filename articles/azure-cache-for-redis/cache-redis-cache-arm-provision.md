---
title: Bir Azure önbelleği için Redis Azure Resource Manager kullanarak sağlama | Microsoft Docs
description: Bir Azure önbelleği için Redis dağıtmak için Azure Resource Manager şablonu kullanın.
services: app-service
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: yegu
ms.openlocfilehash: 5bdad61df732f0aeb1a758aacb5844204387e19b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66132784"
---
# <a name="create-an-azure-cache-for-redis-using-a-template"></a>Şablon kullanarak Redis için bir Azure önbelleği oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu konu başlığında, bir Azure önbelleği için Redis dağıtan bir Azure Resource Manager şablonunun nasıl oluşturulacağını öğrenin. Önbellek ile mevcut bir depolama hesabı Tanılama verileri tutmak için kullanılabilir. Ayrıca tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen nasıl öğrenin. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şu anda, tanılama ayarları için bir abonelik için aynı bölgede tüm önbellekleri paylaşılır. Bir önbellek bölgede güncelleştirme bölgesindeki tüm önbellekleri etkiler.

Şablonları oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md). Önbellek kaynak türleri için özellikleri ve JSON söz dizimi hakkında bilgi edinmek için bkz. [Microsoft.Cache kaynak türleri](/azure/templates/microsoft.cache/allversions).

Tam şablon için bkz: [Azure önbelleği için Redis şablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Yeni Resource Manager şablonları [Premium katmanı](cache-premium-tier-intro.md) kullanılabilir. 
> 
> * [Premium Azure önbelleği için Redis Kümeleme ile oluşturma](https://azure.microsoft.com/resources/templates/201-redis-premium-cluster-diagnostics/)
> * [Premium Azure önbelleği için Redis veri kalıcılığı ile oluşturun.](https://azure.microsoft.com/resources/templates/201-redis-premium-persistence/)
> * [Dağıtılan bir sanal ağa Premium Redis önbelleği oluşturma](https://azure.microsoft.com/resources/templates/201-redis-premium-vnet/)
> 
> En yeni şablonları denetlemek için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/) araması `Azure Cache for Redis`.
> 
> 

## <a name="what-you-will-deploy"></a>Ne dağıtacaksınız
Bu şablonda mevcut bir depolama hesabı için Tanılama verileri kullanan Redis için bir Azure önbelleği dağıtır.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon parametreleri olarak adlandırılan bir tüm parametre değerlerini içeren bölüm içerir.
Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama göre değişen değerler için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
Azure Cache, Redis için konumu. En iyi performans için önbellek ile kullanılmak üzere bir uygulama olarak aynı konumu kullanın.

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
### <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Azure önbelleği için Redis oluşturur.

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

    New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure CLI'si
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


