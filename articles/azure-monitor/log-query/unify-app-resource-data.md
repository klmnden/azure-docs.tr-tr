---
title: Birden çok Azure İzleyici Application Insights kaynaklarını birleştirin | Microsoft Docs
description: Bu makalede, bir işlevi Azure İzleyici günlüklerine birden fazla Application Insights kaynaklarını sorgulama ve bu verileri görselleştirmek için kullanılacağı hakkında ayrıntılar sağlar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: magoedte
ms.openlocfilehash: 1dba84c686fbb873f044b4980990baa396a94c79
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237680"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Birden çok Azure İzleyici Application Insights kaynaklarını birleştirin 
Bu makalede, sorgu ve farklı Azure aboneliklerinde Application Insights Bağlayıcısı kullanımdan bir ardılı olarak olduklarında bile tüm Application Insights uygulama günlük verilerini tek bir yerde görüntüleyin açıklar. 100 olarak kaynakları tek bir sorgu ekleyebilirsiniz, Application Insights kaynakları sayısı sınırlıdır.  

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Önerilen yaklaşım, birden fazla Application Insights kaynaklarını sorgulama 
Bir sorguda birden fazla Application Insights kaynakları listeleme hantal ve sürdürülmesi zor olabilir. Bunun yerine, sorgu mantığının kapsamı uygulamalardan ayırmak için işlev yararlanabilirsiniz.  

Bu örnek nasıl birden fazla Application Insights kaynaklarını izleyebilir ve uygulama adına göre başarısız istek sayısını görselleştirmek gösterir. Başlamadan önce bağlı uygulamalar listesini almak için Application Insights kaynağı bağlı çalışma alanında bu sorguyu çalıştırın: 

```
ApplicationInsights
| summarize by ApplicationName
```

Uygulamaların listesini birleşim işleci kullanarak bir işlev oluşturun ve ardından sorguyu işlev diğer adı ile çalışma alanınızda Kaydet *applicationsScoping*.  

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

>[!NOTE]
>Listelenen uygulamalar portalında herhangi bir zamanda çalışma alanınızdaki sorgu Gezgini giderek ve düzenleme ve ardından kaydetme ya da kullanarak işlevi seçerek değiştirebileceğiniz `SavedSearch` PowerShell cmdlet'i. `withsource= SourceApp` Komutu sonuçları uygulama belirten bir sütuna gönderilen günlük ekler. 
>
>Application Insights veri yapısı applicationsScoping işlevi döndürdüğünden çalışma alanında sorgu yürütülür, ancak sorgu Application Insights şema kullanılır. 
>
>Ayrıştırma işleci Bu örnekte, isteğe bağlı, uygulama adı SourceApp özelliğinden ayıklar. 

Kaynaklar arası sorgu applicationsScoping işlevi kullanmak artık hazırsınız:  

```
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

İşlev diğer adı, tanımlanan tüm uygulamalardan istekleri birleşimini döndürür. Sorgu daha sonra başarısız olan istekleri için filtreler ve uygulama tarafından eğilimleri görselleştirir.

![Çapraz-sorgu sonuçlarını örneği](media/unify-app-resource-data/app-insights-query-results.png)

## <a name="query-across-application-insights-resources-and-workspace-data"></a>Application Insights kaynaklarını ve çalışma alanı veri sorgulama 
Bağlayıcı ve Application Insights veri saklama (90 gün) tarafından kırpılmış bir zaman aralığı boyunca sorguları gerçekleştirmek için ihtiyaç durdurduğunuzda, gerçekleştirmeniz gereken [kaynaklar arası sorgular](../../azure-monitor/log-query/cross-workspace-query.md) çalışma ve Application Insights bir ara dönem için kaynaklar. Yukarıda belirtilen yeni Application Insights veri saklama başına uygulama verilerinize biriktirir kadar budur. Application Insights ve çalışma alanı şemalarda farklı olduğundan sorgu bazı işlemeleri gerektirir. Daha sonra şema farklılıkları vurgulama bu bölümdeki tabloya bakın. 

Örneğin, bağlayıcı, Application Insights kaynaklarını ve uygulamaların veri çalışma boyunca günlükleri sorguladığınızda, 2018-11-01 üzerinde çalışmayı durdurdu, sorgunuzu aşağıdaki örnekteki gibi oluşturulması:

```
applicationsScoping //this brings data from Application Insights resources 
| where timestamp between (datetime("2018-11-01") .. now()) 
| where success == 'False' 
| where duration > 1000 
| union ( 
    ApplicationInsights //this is Application Insights data in Log Analytics workspace 
    | where TimeGenerated < (datetime("2018-12-01") 
    | where RequestSuccess == 'False' 
    | where RequestDuration > 1000 
    | extend duration = RequestDuration //align to Application Insights schema 
    | extend timestamp = TimeGenerated //align to Application Insights schema 
    | extend name = RequestName //align to Application Insights schema 
    | extend resultCode = ResponseCode //align to Application Insights schema 
    | project-away RequestDuration , RequestName , ResponseCode , TimeGenerated 
) 
| project timestamp , duration , name , resultCode 
```

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Application Insights ve Log Analytics çalışma alanı şema farklılıkları
Aşağıdaki tabloda, Log Analytics ve Application Insights şema farklılıkları gösterir.  

| Günlük analizi çalışma alanı özellikleri| Application Insights kaynak özellikleri|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationId | appId|
| ApplicationName | AppName|
| ApplicationTypeVersion | application_Version |
| AvailabilityCount | ItemCount |
| AvailabilityDuration | süre |
| AvailabilityMessage | message |
| AvailabilityRunLocation | location |
| AvailabilityTestId | id |
| AvailabilityTestName | ad |
| AvailabilityTimestamp | timestamp |
| Tarayıcı | client_browser |
| Şehir | client_city |
| Clientıp | client_IP |
| Bilgisayar | cloud_RoleInstance | 
| Ülke | client_CountryOrRegion | 
| CustomEventCount | ItemCount | 
| CustomEventDimensions | customDimensions |
| CustomEventName | ad | 
| DeviceModel | client_Model | 
| DeviceType | client_Type | 
| ExceptionCount | ItemCount | 
| ExceptionHandledAt | handledAt |
| ExceptionMessage | message | 
| ExceptionType | type |
| Operationıd | operation_ıd |
| OperationName | operation_Name | 
| İşletim Sistemi | client_OS | 
| PageViewCount | ItemCount |
| PageViewDuration | süre | 
| PageViewName | ad | 
| ParentOperationID | operation_ıd | 
| RequestCount | ItemCount | 
| RequestDuration | süre | 
| RequestId | id | 
| RequestName | ad | 
| RequestSuccess | başarılı | 
| Yanıt kodu | ResultCode | 
| Rol | cloud_RoleName |
| Roleınstance | cloud_RoleInstance |
| oturum kimliği | session_ıd | 
| SourceSystem | operation_SyntheticSource |
| TelemetryTYpe | type |
| URL'si | _url |
| UserAccountId | user_AccountId |

## <a name="next-steps"></a>Sonraki adımlar

Kullanım [günlük araması](../../azure-monitor/log-query/log-query-overview.md) Application Insights uygulamalarınız için ayrıntılı bilgileri görüntülemek için.
