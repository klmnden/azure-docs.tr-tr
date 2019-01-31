---
title: Azure Log Analytics ile kaynakları genelinde arama yapma | Microsoft Docs
description: Bu makalede nasıl birden çok çalışma alanları ve App Insights uygulama kaynaklarda aboneliğinizde Sorgulayabileceğiniz açıklanır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: magoedte
ms.openlocfilehash: 42191b21faec7bb1929a12e6bc1a724d269acb1d
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55298883"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Log Analytics'te kaynaklar arası günlük aramaları gerçekleştirme  

Daha önce Azure Log Analytics ile yalnızca geçerli çalışma alanındaki verileri çözümleyebilir ve aboneliğinizde tanımlanmış birden çok çalışma alanında sorgu yapabilmenizi sınırlıdır.  Ayrıca, Application ınsights'ta doğrudan Application Insights ile web tabanlı uygulamanızın içinden veya Visual Studio'dan toplanan telemetri öğelerinin yalnızca arama yapabilirsiniz.  Bu ayrıca yerel olarak operasyonel analiz etmek için sınama ve uygulama verileri birlikte hale.   

Artık yalnızca birden fazla Log Analytics çalışma alanları, aynı zamanda aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir Application Insights uygulamasından verileriniz üzerinde sorgulama yapabilirsiniz. İle verilerinizin sistem genelinde bir görünüm sağlar.  Yalnızca bu tür bir sorgu gerçekleştirebileceğiniz [Log Analytics](portals.md#log-analytics-page). 100'e (Log Analytics çalışma alanları ve Application Insights uygulaması), tek bir sorguda dahil edebileceğiniz kaynak sayısı sınırlıdır. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Application ınsights ve Log Analytics çalışma alanları arasında sorgulama
Sorgunuzu başka bir çalışma alanı başvurmak için kullanma [ *çalışma* ](https://docs.microsoft.com/azure/log-analytics/query-language/workspace-expression) tanımlayıcısı ve Application ınsights'tan bir uygulama için [ *uygulama* ](https://docs.microsoft.com/azure/log-analytics/query-language/app-expression)tanımlayıcısı.  

### <a name="identifying-workspace-resources"></a>Çalışma alanı kaynakları tanımlama
Aşağıdaki örnekler, günlükleri özetlenen sayısını güncelleştirme tablosundan adlı bir çalışma alanına dönmek için Log Analytics çalışma alanları arasında sorguları gösterir. *contosoretail BT*. 

Bir çalışma alanı tanımlama yollarından biri kullanılarak gerçekleştirilebilir olabilir:

* Kaynak adı - bazen denir, çalışma alanının okunabilir bir ad olduğu *bileşen adı*. 

    `workspace("contosoretail-it").Update | count`
 
    >[!NOTE]
    >Bir çalışma alanı adına göre tanımlayan benzersizlik tüm erişilebilir aboneliklerindeki varsayar. Belirtilen ada sahip birden çok uygulamalarınız varsa, sorgu belirsizlik nedeniyle başarısız olur. Bu durumda, diğer tanımlayıcılarla birini kullanmanız gerekir.

* Tam adı - abonelik adı, kaynak grubu ve bileşen adı şu biçimde oluşan çalışma alanında, "tam adı" olduğunu: *resourceGroup/subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/contosoretail-it').Update | count `

    >[!NOTE]
    >Azure aboneliği adları benzersiz olmuyor çünkü bu tanımlayıcısı belirsiz olabilir. 
    >

* Çalışma alanı kimliği - atanan genel benzersiz tanıtıcısı (GUID) temsil edilen her bir çalışma alanı için benzersiz, sabit, tanımlayıcı bir çalışma alanı kimliğidir.

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure Resource ID: çalışma alanının Azure tanımlı benzersiz kimliği. Kaynak adı belirsiz olduğunda kaynak Kimliğini kullanın.  Çalışma alanları için biçimi şu şekildedir: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Çalışma alanları/Operationalınsights/componentName*.  

    Örneğin:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail-it").Update | count
    ```

### <a name="identifying-an-application"></a>Bir uygulama tanımlama
Aşağıdaki örnekte adlı bir uygulama karşı yapılan istekleri özetlenmiş bir sayısını döndürmek *fabrikamapp* Application ınsights. 

İle bir Application Insights uygulamasında tanımlama gerçekleştirilebilir *app(Identifier)* ifade.  *Tanımlayıcı* bağımsız değişkeni, aşağıdakilerden birini kullanarak uygulamayı belirtir:

* -Kaynak adı olduğundan bazen denir, app insan tarafından okunabilir adını *bileşen adı*.  

    `app("fabrikamapp")`

* Tam adı - addır "tam" uygulamasının abonelik adı, kaynak grubu ve bileşen adı şu biçimde oluşur: *resourceGroup/subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Azure aboneliği adları benzersiz olmuyor çünkü bu tanımlayıcısı belirsiz olabilir. 
    >

* Kimliği - uygulama uygulamasının GUID.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure kaynak kimliği - uygulamayı Azure tanımlı benzersiz kimliği. Kaynak adı belirsiz olduğunda kaynak Kimliğini kullanın. Biçim: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Operationalınsights/bileşenleri/componentName*.  

    Örneğin:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

### <a name="performing-a-query-across-multiple-resources"></a>Birden fazla kaynak arasında sorgu gerçekleştirme
Birden çok kaynak kaynak örnekleriniz hiçbirini sorgulayabilir, bunlar çalışma alanları ve uygulamaları olabilir birleştirilmiş.
    
Örneğin, iki çalışma alanları arasında sorgu:    

```
union Update, workspace("contosoretail-it").Update, workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update
| where TimeGenerated >= ago(1h)
| where UpdateState == "Needed"
| summarize dcount(Computer) by Classification
```

## <a name="using-cross-resource-query-for-multiple-resources"></a>Kaynaklar arası sorgu için birden çok kaynakları kullanma
Kaynaklar arası sorgular birden fazla Log Analytics ve Application Insights kaynakları verileri ilişkilendirmek için kullanılırken, sorgu karmaşık ve sürdürülmesi zor hale gelebilir. Faydalanın [Log analytics'te işlevleri](../../azure-monitor/log-query/functions.md) kapsamı sorgu yapısı basitleştiren sorgu kaynaklarından gelen sorgu mantığının ayırmak için. Aşağıdaki örnek nasıl birden fazla Application Insights kaynaklarını izleyebilir ve uygulama adına göre başarısız istek sayısını görselleştirmek gösterir. 

Application Insights kaynakları kapsamını başvuran aşağıdaki gibi bir sorgu oluşturun. `withsource= SourceApp` Komut, uygulama adını belirten bir sütun gönderilen günlük ekler. [İşlevi sorguyu kaydetmek](../../azure-monitor/log-query/functions.md#create-a-function) takma ad ile _applicationsScoping_.

```Kusto
// crossResource function that scopes my Application Insights resources
union withsource= SourceApp
app('Contoso-app1').requests, 
app('Contoso-app2').requests,
app('Contoso-app3').requests,
app('Contoso-app4').requests,
app('Contoso-app5').requests
```



Artık [bu işlevi kullanın](../../azure-monitor/log-query/functions.md#use-a-function) kaynaklar arası sorguda aşağıdaki gibi. İşlev diğer adı _applicationsScoping_ tanımlanan tüm uygulamalardan requests tablosuna birleşimini döndürür. Sorgu daha sonra başarısız olan istekleri için filtreler ve uygulama tarafından eğilimleri görselleştirir. _Ayrıştırma_ işleci, bu örnekte isteğe bağlıdır. Uygulama adından ayıklar _SourceApp_ özelliği.

```Kusto
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```
![zaman grafiğini](media/cross-workspace-query/chart.png)

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [Log Analytics günlük araması başvuru](https://docs.microsoft.com/azure/log-analytics/query-language/kusto) tüm kullanılabilir Log Analytics'te sorgu söz dizimi seçeneklerini görüntüleyin.    
