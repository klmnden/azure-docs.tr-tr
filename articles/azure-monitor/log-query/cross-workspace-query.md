---
title: Azure İzleyici ile kaynak arasında sorgu | Microsoft Docs
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
ms.date: 06/05/2019
ms.author: magoedte
ms.openlocfilehash: 5e411182a26e370ef82a20e67ee18cedd5d96d86
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296103"
---
# <a name="perform-cross-resource-log-queries-in-azure-monitor"></a>Azure İzleyici'de kaynaklar arası günlük sorguları gerçekleştirme  

Daha önce Azure İzleyici ile yalnızca geçerli çalışma alanındaki verileri çözümleyebilir ve aboneliğinizde tanımlanmış birden çok çalışma alanında sorgu yapabilmenizi sınırlıdır.  Ayrıca, Application ınsights'ta doğrudan Application Insights ile web tabanlı uygulamanızın içinden veya Visual Studio'dan toplanan telemetri öğelerinin yalnızca arama yapabilirsiniz. Bu ayrıca yerel olarak operasyonel analiz etmek için sınama ve uygulama verileri birlikte hale.

Artık yalnızca birden fazla Log Analytics çalışma alanları, aynı zamanda aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir Application Insights uygulamasından verileriniz üzerinde sorgulama yapabilirsiniz. İle verilerinizin sistem genelinde bir görünüm sağlar. Yalnızca bu tür bir sorgu gerçekleştirebileceğiniz [Log Analytics](portals.md).

## <a name="cross-resource-query-limits"></a>Kaynaklar arası sorgu sınırları 

* Application Insights kaynaklarını ve Log Analytics çalışma alanları, tek bir sorguda dahil edebileceğiniz sayısı 100'e sınırlıdır.
* Kaynaklar arası sorgu görünümü Tasarımcısı'nda desteklenmiyor. Log analytics'te sorgu yazar ve sabitleyebilirsiniz Azure panosuna için [günlük sorgusu görselleştirme](../learn/tutorial-logs-dashboards.md). 
* Kaynaklar arası sorgu günlüğü uyarıları yeni desteklenen [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Varsayılan olarak, Azure İzleyici kullanır [eski Log Analytics uyarı API](../platform/api-alerts.md) gelen geçiş yapmadığınız sürece, yeni günlük uyarı kuralları Azure portalından oluşturmak için [eski günlük uyarıları API](../platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api). Anahtardan sonra yeni API, Azure portalında yeni uyarı kuralları için varsayılan olur ve kaynaklar arası sorgu günlük uyarı kuralları oluşturmanıza olanak tanır. Kullanarak anahtarı yapmadan kaynaklar arası sorgu günlüğü uyarı kuralları oluşturabilirsiniz [scheduledQueryRules API'si için Azure Resource Manager şablonu](../platform/alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) – ancak yine de bu uyarı kuralı yönetilebilir [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) ve Azure Portal'da değil.


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

    `workspace('contoso/contosoretail/contosoretail-it').Update | count`

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
Kaynaklar arası sorgular birden fazla Log Analytics çalışma alanları ve Application Insights kaynakları verileri ilişkilendirmek için kullanılırken, sorgu karmaşık ve sürdürülmesi zor hale gelebilir. Faydalanın [işlevleri Azure İzleyici'de oturum sorguları](functions.md) kapsamı sorgu yapısı basitleştiren sorgu kaynaklarından gelen sorgu mantığının ayırmak için. Aşağıdaki örnek nasıl birden fazla Application Insights kaynaklarını izleyebilir ve uygulama adına göre başarısız istek sayısını görselleştirmek gösterir. 

Application Insights kaynakları kapsamını başvuran aşağıdaki gibi bir sorgu oluşturun. `withsource= SourceApp` Komut, uygulama adını belirten bir sütun gönderilen günlük ekler. [İşlevi sorguyu kaydetmek](functions.md#create-a-function) takma ad ile _applicationsScoping_.

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

- Gözden geçirme [analiz günlük verilerini Azure İzleyici'de](log-query-overview.md) günlük sorguları ve Azure İzleyici günlük verilerini nasıl yapılandırıldığını genel bakış.
- Gözden geçirme [Azure İzleyici günlük sorguları](query-language.md) tüm kaynaklar için Azure İzleyici günlük sorguları görüntüleyin.
