---
title: "Azure günlük analizi çalışma alanları arasında sorguları gerçekleştirmek | Microsoft Docs"
description: "Bu makalede, aboneliğinizde nasıl birden çok çalışma alanları ve belirli App Insights uygulama arasında sorgulayabilirsiniz açıklanmaktadır."
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
ms.date: 02/15/2018
ms.author: magoedte
ms.openlocfilehash: 403448995c28ff7172d2c3abbf3b9d67341017b4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-perform-queries-across-multiple-log-analytics-workspaces"></a>Birden çok günlük analizi çalışma alanları arasında sorgular gerçekleştirme

Daha önce Azure günlük analizi ile yalnızca geçerli çalışma alanı içindeki verileri çözümleyebilir ve aboneliğinizde tanımlanan birden çok çalışma alanları arasında becerinizi sorgu sınırlıdır.  

Artık yalnızca birden çok günlük analizi çalışma alanı, aynı zamanda belirli bir Application Insights uygulamayı aynı kaynak grubu, başka bir kaynak grubu veya başka bir abonelik verileri arasında sorgulayabilirsiniz. Bu, verilerinizi bir sistem genelinde görünümünü sağlar.  Bu tür bir sorguda yalnızca gerçekleştirebilirsiniz [Gelişmiş portal](log-analytics-log-search-portals.md#advanced-analytics-portal), Azure portalında değil.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Günlük analizi çalışma alanları arasında ve Application Insights sorgulama
Sorgunuzdaki başka bir çalışma alanı başvurmak için [ *çalışma* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) tanımlayıcısı ve bir uygulamadan için Application Insights, [ *uygulama* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())tanımlayıcısı.  

Örneğin, ilk sorguyu sınıflandırmalarına hem geçerli çalışma alanı hem de başka bir çalışma alanı adlı güncelleştirme tablosundan gerekli güncelleştirmelerin özetlenen sayıları döndürür *contosoretail BT*.  İkinci sorgu örnek adlı bir uygulama karşı yapılan istekleri özetlenen sayımını döndürür *fabrikamapp* Application ınsights'ta. 

### <a name="identifying-workspace-resources"></a>Çalışma alanı kaynakları tanımlama
Bir çalışma alanı tanımlayan çeşitli yollardan gerçekleştirilebilir:

* Kaynak adı - adıdır bir okunabilir bazen denir çalışma alanında, *bileşen adı*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Bir çalışma alanı adıyla tanımlayan erişilebilir abonelikler arasında benzersiz olduğunu varsayar. Belirtilen ada sahip birden çok uygulamalarınız varsa, sorgu belirsizlik nedeniyle başarısız olur. Bu durumda, diğer tanımlayıcıları birini kullanmanız gerekir.

* Koşullu - adıdır abonelik adı, kaynak grubu ve bileşen adı bu biçimde oluşan çalışma alanında, "tam adı": *resourceGroup/varlığıyla subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Azure aboneliği adlarının benzersiz olduğundan bu tanımlayıcısı belirsiz olabilir. 
    >

* Çalışma alanı kimliği - çalışma alanı kimliği için bir genel benzersiz tanımlayıcı (GUID) olarak gösterilen her çalışma alanına atanan benzersiz, sabit, tanımlayıcısıdır.

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure kaynak kimliği – çalışma alanının Azure tarafından tanımlanan benzersiz kimlik. Kaynak adı belirsiz olduğunda kaynak kimliği kullanın.  Çalışma alanları için biçimi şöyledir: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Çalışma alanları/OperationalInsights/componentName*.  

    Örneğin:
    ``` 
    workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Bir uygulama tanımlama

İle Application Insights uygulamada tanımlayan uygulanabilir *app(Identifier)* ifade.  *Tanımlayıcısı* bağımsız değişkeni şunlardan birini kullanarak uygulamayı belirtir:

* Kaynak adı - adıdır bir insan okunabilir bazen denir uygulama *bileşen adı*.  

    `app("fabrikamapp")`

* Koşullu - adıdır uygulamasının abonelik adı, kaynak grubu ve bileşen adı bu biçimde oluşan "tam adı": *resourceGroup/varlığıyla subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Azure aboneliği adlarının benzersiz olduğundan bu tanımlayıcısı belirsiz olabilir. 
    >

* Kimliği - uygulama uygulamanın GUID'si.

    `app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count`

* Azure kaynak kimliği - uygulamanın Azure tarafından tanımlanan benzersiz kimlik. Kaynak adı belirsiz olduğunda kaynak kimliği kullanın. Biçim: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Bileşenleri/OperationalInsights/componentName*.  

    Örneğin:
    ```
    app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [günlük analizi oturum Arama başvurusu](https://docs.loganalytics.io/docs/Language-Reference) sorgu sözdizimi seçenekleri günlük analizi görüntülemek için.    