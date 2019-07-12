---
title: Azure Klasik dağıtım kaynakları yeni abonelik veya kaynak grubuna taşıma
description: Klasik dağıtım kaynakları yeni kaynak grubuna veya aboneliğe taşıma için Azure Resource Manager'ı kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 4770f957b6b9eea75b50776a7491b1ca479e50e2
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723512"
---
# <a name="move-guidance-for-classic-deployment-model-resources"></a>Klasik dağıtım modeli kaynakları için Taşıma Kılavuzu

Klasik modelle dağıtılmış kaynakları taşıma adımları kaynakları bir abonelik içinde veya yeni bir aboneliğe taşıma bağlı olarak farklılık gösterir.

## <a name="move-in-the-same-subscription"></a>Aynı abonelikte Taşı

Aynı abonelik içindeki başka bir kaynak grubu için bir kaynak grubundan kaynakları taşırken aşağıdaki kısıtlamalar uygulanır:

* Sanal ağlar (Klasik) taşınamaz.
* Sanal makineler (Klasik) bulut hizmeti ile taşınmalıdır.
* Bulut hizmeti, tüm sanal makineleri taşıma içerdiğinde yalnızca taşınabilir.
* Bir kerede yalnızca bir bulut hizmeti taşınabilir.
* Bir kerede yalnızca bir depolama hesabı (Klasik) taşınabilir.
* Depolama hesabı (Klasik), bir sanal makine veya Bulut hizmeti ile aynı işlemde taşınamaz.

Klasik kaynakları aynı abonelik içindeki yeni bir kaynak grubuna taşımak için kullanın [standart taşıma işlemlerini](../resource-group-move-resources.md) portal, Azure PowerShell, Azure CLI veya REST API aracılığıyla. Resource Manager kaynaklarını taşımak için kullandığınız gibi işlemlerin aynısını kullanın.

## <a name="move-across-subscriptions"></a>Abonelikler arasında taşıma

Kaynakları yeni bir aboneliğe taşınmasını, aşağıdaki kısıtlamalar uygulanır:

* Abonelikteki tüm Klasik kaynaklar aynı işlem içinde taşınmalıdır.
* Hedef aboneliği diğer Klasik kaynaklar olmaması gerekir.
* Taşıma, yalnızca klasik taşıma için ayrı bir REST API aracılığıyla istenebilir. Klasik kaynakları için yeni bir abonelik taşırken standart Resource Manager'a taşıma komutlar çalışmaz.

Klasik kaynakları için yeni bir aboneliği taşımak, Klasik kaynakları için özel REST işlemlerini kullanın. REST kullanmak için aşağıdaki adımları uygulayın:

1. Kaynak abonelik bir çapraz abonelik taşıma katılabilir, kontrol edin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunları içerir:

   ```json
   {
    "role": "source"
   }
   ```

     Doğrulama işleminde yanıta aşağıdaki biçimdedir:

   ```json
   {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
   }
   ```

1. Hedef abonelik bir çapraz abonelik taşıma katılabilir, kontrol edin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunları içerir:

   ```json
   {
    "role": "target"
   }
   ```

     Kaynak abonelik doğrulama ile aynı biçimde yanıttır.
1. Her iki abonelik doğrulama testlerini geçerse, tüm Klasik kaynaklar bir abonelikten şu işlemi başka bir aboneliğe Taşı:

   ```HTTP
   POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
   ```

    İstek gövdesinde şunları içerir:

   ```json
   {
    "target": "/subscriptions/{target-subscription-id}"
   }
   ```

İşlemi birkaç dakika çalışabilir.

## <a name="next-steps"></a>Sonraki adımlar

Klasik kaynakları taşıma sorun yaşıyorsanız, kişi [Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
