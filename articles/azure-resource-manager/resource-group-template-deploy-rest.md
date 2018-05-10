---
title: REST API ve şablon kaynaklarla dağıtma | Microsoft Docs
description: Azure Resource Manager ve Resource Manager REST API'si bir kaynakları Azure'a dağıtmak için kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
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
ms.date: 05/01/2018
ms.author: tomfitz
ms.openlocfilehash: bf2fc2aeb094a828fa1efe6904b897f3a4ab46d8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma

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
1. Ayarlama [ortak parametrelerini ve üstbilgileri](/rest/api/azure/), kimlik doğrulama belirteçleri de dahil olmak üzere.

2. Varolan bir kaynak grubu yoksa, bir kaynak grubu oluşturun. Abonelik Kimliğinizi, yeni kaynak grubunu ve çözümünüz için gereksinim duyduğunuz konumu adını sağlayın. Daha fazla bilgi için bkz: [bir kaynak grubu oluşturmak](/rest/api/resources/resourcegroups/createorupdate).

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
  {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
  }
  ```

3. Çalıştırarak çalıştırmadan önce dağıtımınızı doğrulama [şablon dağıtımı doğrulamak](/rest/api/resources/deployments/validate) işlemi. Tam olarak (sonraki adımda gösterilen) dağıtım yürütülürken gibi dağıtım sınarken parametreleri sağlar.

4. Bir dağıtım oluşturun. Abonelik Kimliğiniz, kaynak grubunun adı, dağıtım ve şablonunuzu bağlantı adını sağlayın. Şablon dosyası hakkında daha fazla bilgi için bkz: [parametre dosyası](#parameter-file). Bir kaynak grubu oluşturmak için REST API hakkında daha fazla bilgi için bkz: [şablon dağıtımı oluşturmak](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Bildirim **modu** ayarlanır **artımlı**. Tam dağıtım çalışacak şekilde ayarlanmış **modu** için **tam**. Şablonunuzda olmayan kaynakları yanlışlıkla silebilirsiniz gibi tam modu kullanırken dikkatli olun.

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
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
  ```

    Yanıt içeriğini, istek içeriği veya her ikisi de oturum istiyorsanız dahil **debugSetting** isteği.

  ```HTTP
  "debugSetting": {
    "detailLevel": "requestContent, responseContent"
  }
  ```

    Bir paylaşılan erişim imzası (SAS) belirteci kullanmak üzere depolama hesabınızı ayarlayabilirsiniz. Daha fazla bilgi için bkz: [paylaşılan erişim imzası için temsilci seçme erişimle](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

5. Şablon dağıtımı durumunu alın. Daha fazla bilgi için bkz: [şablon dağıtımı hakkında bilgi alma](/rest/api/resources/deployments/get).

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
  ```

## <a name="redeploy-when-deployment-fails"></a>Dağıtımı başarısız olduğunda yeniden dağıtın

Başarısız dağıtımları için dağıtım geçmişiniz önceki bir dağıtıma otomatik olarak imzalanmasını belirtebilirsiniz. Dağıtımlarınızı bu seçeneği kullanmak için geçmişinde tanımlanan şekilde benzersiz adlara sahip olmalıdır. Benzersiz adlara sahip değilseniz, geçerli başarısız dağıtım geçmişini önceden başarılı dağıtım üzerine yazabilirsiniz. Bu gibi durumlarda, bu seçenek yalnızca kök düzeyinde dağıtımlar kullanabilirsiniz. İç içe geçmiş bir şablondan dağıtımları yeniden dağıtım için kullanılamaz.

Geçerli dağıtım başarısız olursa son başarılı dağıtımı yeniden dağıtmak için kullanın:

```HTTP
"onErrorDeployment": {
  "type": "LastSuccessful",
},
```

Geçerli dağıtım başarısız olursa belirli bir dağıtımı yeniden dağıtmak için kullanın:

```HTTP
"onErrorDeployment": {
  "type": "SpecificDeployment",
  "deploymentName": "<deploymentname>"
}
```

Belirtilen dağıtım başarılı gerekir.

## <a name="parameter-file"></a>Parametre dosyası

Dağıtım sırasında parametre değerleri geçirmek için bir parametre dosyası kullanmak, bir JSON dosyası formatı ile aşağıdaki örneğe benzer şekilde oluşturmanız gerekir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

Parametre dosyanın boyutu 64 KB'den büyük olamaz.

Bir parametre (örneğin, parola) için önemli bir değer sağlamanız gerekiyorsa, bu değer bir anahtar Kasası'na ekleyin. Anahtar kasası, önceki örnekte gösterildiği gibi dağıtım sırasında alın. Daha fazla bilgi için bkz: [dağıtımı sırasında güvenli değerlerini geçirin](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Sonraki adımlar
* Zaman uyumsuz REST işlemlerini işleme hakkında bilgi edinmek için [izlemek zaman uyumsuz Azure işlemleri](resource-manager-async-operations.md).
* .NET istemci kitaplığını kaynaklarına dağıtma ilişkin bir örnek için bkz: [.NET kitaplıkları ve bir şablon kullanarak kaynakları dağıtmak](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
* Çözümünüzü farklı ortamlarda dağıtmaya yönelik kılavuz için bkz. [Microsoft Azure’da geliştirme ve test ortamları](solution-dev-test-environments.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

