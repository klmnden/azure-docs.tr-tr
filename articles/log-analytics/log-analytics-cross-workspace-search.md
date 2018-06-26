---
title: Azure günlük analizi ile kaynakları genelinde arama yapma | Microsoft Docs
description: Bu makalede nasıl birden çok çalışma alanları ve App Insights uygulama kaynaklara karşı aboneliğinizde Sorgulayabileceğiniz açıklanmaktadır.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: magoedte
ms.openlocfilehash: 52c3914cc1b51bf7c2a6d0fbf28dc0bf7756e749
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751454"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Günlük analizi arası kaynak günlük aramalar gerçekleştirmek  

Daha önce Azure günlük analizi ile yalnızca geçerli çalışma alanı içindeki verileri çözümleyebilir ve aboneliğinizde tanımlanan birden çok çalışma alanları arasında becerinizi sorgu sınırlıdır.  Ayrıca, yalnızca Application Insights'ta doğrudan Application Insights ile web tabanlı uygulamanız veya Visual Studio'dan toplanan telemetri öğeleri araması yapabilirsiniz.  Bu da yerel olarak işletimsel çözümlemek için sınama ve uygulama verileri birlikte yapılan.   

Artık yalnızca birden çok günlük analizi çalışma alanı, aynı zamanda belirli bir Application Insights uygulamayı aynı kaynak grubu, başka bir kaynak grubu veya başka bir abonelik verileri arasında sorgulayabilirsiniz. Bu, verilerinizi bir sistem genelinde görünümünü sağlar.  Bu tür sorgularında yalnızca gerçekleştirebilirsiniz [Gelişmiş portal](log-analytics-log-search-portals.md#advanced-analytics-portal), Azure portalında değil. Tek bir sorgu içeren kaynak sayısı (günlük analizi çalışma alanları ve Application Insights uygulama) 100 sınırlıdır. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Günlük analizi çalışma alanları arasında ve Application Insights sorgulama
Sorgunuzdaki başka bir çalışma alanı başvurmak için [ *çalışma* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) tanımlayıcısı ve bir uygulamadan için Application Insights, [ *uygulama* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())tanımlayıcısı.  

### <a name="identifying-workspace-resources"></a>Çalışma alanı kaynakları tanımlama
Aşağıdaki örnekler, güncelleştirmelerinin özetlenen sayıları güncelleştirme tablosundan adlı bir çalışma alanında döndürmek için günlük analizi çalışma alanları arasında sorguları gösterir. *contosoretail BT*. 

Bir çalışma alanı tanımlayan başarılı çeşitli yollardan biri olabilir:

* Kaynak adı - adıdır bir okunabilir bazen denir çalışma alanında, *bileşen adı*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Bir çalışma alanı adına göre tanımlayan benzersizlik erişilebilir abonelikler varsayar. Belirtilen ada sahip birden çok uygulamalarınız varsa, sorgu belirsizlik nedeniyle başarısız olur. Bu durumda, diğer tanımlayıcıları birini kullanmanız gerekir.

* Koşullu - adıdır abonelik adı, kaynak grubu ve bileşen adı bu biçimde oluşan çalışma alanında, "tam adı": *resourceGroup/varlığıyla subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Azure aboneliği adlarının benzersiz olduğundan bu tanımlayıcısı belirsiz olabilir. 
    >

* Çalışma alanı kimliği - çalışma alanı kimliği için bir genel benzersiz tanımlayıcı (GUID) olarak gösterilen her çalışma alanına atanan benzersiz, sabit, tanımlayıcısıdır.

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure kaynak kimliği – çalışma alanının Azure tarafından tanımlanan benzersiz kimlik. Kaynak adı belirsiz olduğunda kaynak kimliği kullanın.  Çalışma alanları için biçimi şöyledir: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Çalışma alanları/OperationalInsights/componentName*.  

    Örneğin:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Bir uygulama tanımlama
Aşağıdaki örnekler adlı bir uygulama karşı yapılan istekleri özetlenen sayısını Döndür *fabrikamapp* Application ınsights'ta. 

İle Application Insights uygulamada tanımlayan gerçekleştirilebilir *app(Identifier)* ifade.  *Tanımlayıcısı* bağımsız değişkeni şunlardan birini kullanarak uygulamayı belirtir:

* Kaynak adı - adıdır bir insan okunabilir bazen denir uygulama *bileşen adı*.  

    `app("fabrikamapp")`

* Koşullu - adıdır uygulamasının abonelik adı, kaynak grubu ve bileşen adı bu biçimde oluşan "tam adı": *resourceGroup/varlığıyla subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Azure aboneliği adlarının benzersiz olduğundan bu tanımlayıcısı belirsiz olabilir. 
    >

* Kimliği - uygulama uygulamanın GUID'si.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure kaynak kimliği - uygulamanın Azure tarafından tanımlanan benzersiz kimlik. Kaynak adı belirsiz olduğunda kaynak kimliği kullanın. Biçim: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Bileşenleri/OperationalInsights/componentName*.  

    Örneğin:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [günlük analizi oturum Arama başvurusu](https://docs.loganalytics.io/docs/Language-Reference) sorgu sözdizimi seçenekleri günlük analizi görüntülemek için.    
