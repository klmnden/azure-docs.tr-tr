---
title: "Azure günlük analizi çalışma alanları arasında sorguları gerçekleştirmek | Microsoft Docs"
description: "Bu makalede, aboneliğinizde izleyin örnekleriyle birden çok çalışma alanları arasında nasıl Sorgulayabileceğiniz açıklanmaktadır."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: magoedte
ms.openlocfilehash: 6b6dc6f472a87178c006301f4f692aeff15e257e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="perform-queries-across-multiple-log-analytics-workspaces"></a>Birden çok günlük analizi çalışma alanları arasında sorguları gerçekleştirmek

Daha önce Azure günlük analizi ile yalnızca geçerli çalışma ve bu sınırlı içindeki verileri becerinizi sorgu aboneliğinizde tanımlanan birden çok çalışma alanları arasında analiz.  

Şimdi, verilerinizin sistem genelinde bir görünümünü sağlayarak birden çok çalışma alanları arasında sorgulayabilirsiniz.  Bu tür bir sorguda yalnızca gerçekleştirebilirsiniz [Gelişmiş portal](log-analytics-log-search-portals.md#advanced-analytics-portal), Azure portalında değil.  

## <a name="querying-across-log-analytics-workspaces"></a>Günlük analizi çalışma alanları arasında sorgulama
Başka bir çalışma alanı Sorgunuzdaki başvurmak için *çalışma* tanımlayıcısı.  Örneğin, aşağıdaki sorguyu sınıflandırmalarına hem geçerli çalışma alanı hem de başka bir çalışma alanı adlı güncelleştirme tablosundan gerekli güncelleştirmelerin özetlenen sayıları döndürür *contosoretail BT*.  


## <a name="identifying-resources"></a>Kaynakları tanımlama
Bir çalışma alanı tanımlayan çeşitli yollardan gerçekleştirilebilir:

* Kaynak adı - adıdır bir okunabilir bazen denir çalışma alanında, *bileşen adı*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Bir çalışma alanı adıyla tanımlayan erişilebilir abonelikler arasında benzersiz olduğunu varsayar. Belirtilen ada sahip birden çok uygulamalarınız varsa, sorgu belirsizlik nedeniyle başarısız olur. Bu durumda diğer tanımlayıcıları birini kullanmanız gerekir.

* Koşullu - adıdır "tam adı" abonelik adını oluşan çalışma alanının kaynak grubu ve bileşen adı şu biçimde: *resourceGroup/varlığıyla subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Azure aboneliği adlarının benzersiz olduğundan bu tanımlayıcısı belirsiz olabilir. 
    >

* Çalışma alanı kimliği - çalışma alanı kimliği için bir genel benzersiz tanımlayıcı (GUID) olarak gösterilen her çalışma alanına atanan benzersiz, sabit, tanımlayıcısıdır.

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure kaynak kimliği – çalışma alanının Azure tarafından tanımlanan benzersiz kimlik. Kaynak adı belirsiz olduğunda kullanın.  Çalışma alanları için biçimi şöyledir: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Çalışma alanları/OperationalInsights/componentName*.  

    Örneğin:
    ``` 
    `workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [günlük analizi oturum Arama başvurusu](https://docs.loganalytics.io/docs/Language-Reference) sorgu sözdizimi seçenekleri günlük analizi görüntülemek için.    