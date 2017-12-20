---
title: "REST API ve şablon kaynaklarla dağıtma | Microsoft Docs"
description: "Azure Resource Manager ve Resource Manager REST API'si bir kaynakları Azure'a dağıtmak için kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
ms.openlocfilehash: 46856a25fb57bb2c5a3c1aeae13c11655e1a58a5
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Bu makalede Resource Manager REST API Resource Manager şablonları ile kaynakları Azure'a dağıtmak için nasıl kullanılacağı açıklanmaktadır.  

> [!TIP]
> Dağıtım sırasında bir hata ayıklama daha fazla yardım için bkz:
> 
> * [Dağıtım işlemlerini görüntülemek](resource-manager-deployment-operations.md) , hata gidermenize yardımcı olacak bilgileri alma hakkında bilgi için
> * [Kaynakları Azure Azure Resource Manager ile dağıtırken sık karşılaşılan sorunları giderme](resource-manager-common-deployment-errors.md) genel dağıtım hatalarını gidermek öğrenmek için
> 
> 

Şablonunuz, yerel bir dosya veya bir URI kullanılabilir olan dış dosyası olabilir. Şablonunuzu bir depolama hesabında bulunduğunda, şablona erişimi kısıtlayabilir ve dağıtım sırasında bir paylaşılan erişim imzası (SAS) belirteci sağlayın.

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>REST API ile dağıtma
1. Ayarlama [ortak parametrelerini ve üstbilgileri](https://docs.microsoft.com/rest/api/index), kimlik doğrulama belirteçleri de dahil olmak üzere.
2. Varolan bir kaynak grubu yoksa, bir kaynak grubu oluşturun. Abonelik Kimliğinizi, yeni kaynak grubunu ve çözümünüz için gereksinim duyduğunuz konumu adını sağlayın. Daha fazla bilgi için bkz: [bir kaynak grubu oluşturmak](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. Çalıştırarak çalıştırmadan önce dağıtımınızı doğrulama [şablon dağıtımı doğrulamak](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) işlemi. Tam olarak (sonraki adımda gösterilen) dağıtım yürütülürken gibi dağıtım sınarken parametreleri sağlar.
4. Bir dağıtım oluşturun. Abonelik Kimliğiniz, kaynak grubunun adı, dağıtım ve şablonunuzu bağlantı adını sağlayın. Şablon dosyası hakkında daha fazla bilgi için bkz: [parametre dosyası](#parameter-file). Bir kaynak grubu oluşturmak için REST API hakkında daha fazla bilgi için bkz: [şablon dağıtımı oluşturmak](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Bildirim **modu** ayarlanır **artımlı**. Tam dağıtım çalışacak şekilde ayarlanmış **modu** için **tam**. Şablonunuzda olmayan kaynakları yanlışlıkla silebilirsiniz gibi tam modu kullanırken dikkatli olun.
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0"
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0"
              }
            }
          }
   
      Yanıt içeriğini, istek içeriği veya her ikisi de oturum istiyorsanız dahil **debugSetting** isteği.
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      Bir paylaşılan erişim imzası (SAS) belirteci kullanmak üzere depolama hesabınızı ayarlayabilirsiniz. Daha fazla bilgi için bkz: [paylaşılan erişim imzası için temsilci seçme erişimle](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).
5. Şablon dağıtımı durumunu alın. Daha fazla bilgi için bkz: [şablon dağıtımı hakkında bilgi alma](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a>Parametre dosyası

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Zaman uyumsuz REST işlemlerini işleme hakkında bilgi edinmek için [izlemek zaman uyumsuz Azure işlemleri](resource-manager-async-operations.md).
* .NET istemci kitaplığını kaynaklarına dağıtma ilişkin bir örnek için bkz: [.NET kitaplıkları ve bir şablon kullanarak kaynakları dağıtmak](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
* Çözümünüzü farklı ortamlarda dağıtmaya yönelik kılavuz için bkz. [Microsoft Azure’da geliştirme ve test ortamları](solution-dev-test-environments.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

