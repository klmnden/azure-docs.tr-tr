---
title: Azure İzleyici'de standart özellikler günlük kayıtlarının | Microsoft Docs
description: Azure İzleyici günlüklerine birden çok veri türü için ortak olan özellikleri açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/20/2019
ms.author: bwren
ms.openlocfilehash: 50804e1f6ab4f352239d3f405e5b41e4e0c58d14
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67292815"
---
# <a name="standard-properties-in-azure-monitor-logs"></a>Azure İzleyici günlüklerine standart özellikler
Azure İzleyici günlüklerine verilerde [kümesi bir Log Analytics çalışma alanı veya Application Insights uygulama kayıtları olarak depolanan](../log-query/logs-structure.md), her bir özellik kümesi olan belirli veri türüne sahip. Birçok veri türleri, birden çok türlerinde ortak olan standart özellikleri olacaktır. Bu makalede, bu özellikleri açıklar ve nasıl bunları sorgularında kullanabileceğiniz örnekler sağlar.

Bazı veri türleri, ancak henüz diğerleri bunları görebilirsiniz bu özelliklerin bazıları hala uygulanan sürecinde, olduğundan.

## <a name="timegenerated-and-timestamp"></a>TimeGenerated ve zaman damgası
**TimeGenerated** (Log Analytics çalışma alanı) ve **zaman damgası** kaydın oluşturulduğu saat ve tarihi (Application Insights uygulaması) özellikleri içerir. Bu, filtreleme veya zamana göre özetlemek için kullanılacak ortak bir özellik sağlar. Azure portalında bir zaman aralığı için bir görünüm veya Panoda seçtiğinizde, sonuçları filtrelemek için TimeGenerated veya zaman damgası kullanır.

### <a name="examples"></a>Örnekler

Aşağıdaki sorgu, önceki haftanın her günü için oluşturulan hata olay sayısını döndürür.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

Aşağıdaki sorgu, önceki haftanın her günü için oluşturulan özel durumların sayısını döndürür.

```Kusto
exceptions
| where timestamp between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by timestamp asc 
```

## <a name="type-and-itemtype"></a>Tür ve Itemtype
**Türü** (Log Analytics çalışma alanı) ve **Itemtype** (Application Insights uygulaması) özellikleri askıya kaydı Ayrıca hangi can alınmıştır tablonun adını düşündüğünüz kayıt olarak yazın. Bu özellik, kayıtları kullananlar gibi birden çok tablodan birleştirmek sorgularda yararlıdır `search` farklı türlerde kayıtlar arasında ayrım yapmak için işleci. **$table** yerine kullanılan **türü** bazı yerlerde.

### <a name="examples"></a>Örnekler
Aşağıdaki sorgu, geçtiğimiz saat içinde toplanan türüne göre kayıt sayısını döndürür.

```Kusto
search * 
| where TimeGenerated > ago(1h)
| summarize count() by Type
```

## <a name="resourceid"></a>\_ResourceId
**\_ResourceId** özelliği kaydı ile ilişkili kaynak için benzersiz bir tanımlayıcı içerir. Bu, sorgunuzu kayıtlarına yalnızca belirli bir kaynaktan kapsam veya ilgili verileri birden çok tabloda katılmak için kullanılacak bir standart özelliği sağlar.

Değerini, Azure kaynakları için **_ResourceId** olduğu [Azure kaynak kimliği URL](../../azure-resource-manager/resource-group-template-functions-resource.md). Şu anda Azure kaynaklarına sınırlı bir özelliğidir, ancak şirket içi bilgisayarlar gibi Azure dışındaki kaynaklar için genişletilir.

> [!NOTE]
> Bazı veri türleri içeren Azure kaynak Kimliğini veya en az parça alan zaten var, abonelik kimliği gibi Bu alanlar, geriye dönük uyumluluk için tutulur, ancak daha tutarlı olduğundan çapraz korelasyon gerçekleştirmek için _ResourceId kullanmak için önerilir.

### <a name="examples"></a>Örnekler
Aşağıdaki sorguda her bilgisayar için performans ve olay verileri birleştirir. Kimliğine sahip tüm olaylar gösterir _101_ ve işlemci kullanımı % 50 üzerindeki.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

Aşağıdaki Sorguda birleştirme _AzureActivity_ kayıtlar _SecurityEvent_ kaydeder. Bu, tüm etkinlik işlemleri bu makinelerde oturum açmış kullanıcıları gösterir.

```Kusto
AzureActivity 
| where  
    OperationName in ("Restart Virtual Machine", "Create or Update Virtual Machine", "Delete Virtual Machine")  
    and ActivityStatus == "Succeeded"  
| join kind= leftouter (    
   SecurityEvent 
   | where EventID == 4624  
   | summarize LoggedOnAccounts = makeset(Account) by _ResourceId 
) on _ResourceId  
```

Aşağıdaki sorguda ayrıştırıyor **_ResourceId** ve toplamalar veri hacimleri Azure aboneliği başına faturalandırılır.

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last 
```

Bu `union withsource = tt *` veri türlerinde taramaları çalıştırmak pahalı olduğundan tutumlu sorgular.

## <a name="isbillable"></a>\_IsBillable
**\_IsBillable** özelliği, içe alınan veri Faturalanabilir olup olmadığını belirtir. Verilerle  **\_IsBillable** eşit _false_ ücretsiz toplanan ve Azure hesabınızda faturalandırılmaz.

### <a name="examples"></a>Örnekler
Faturalandırılan veri türleri gönderme bilgisayarların listesini almak için aşağıdaki sorguyu kullanın:

> [!NOTE]
> İle sorguları kullanma `union withsource = tt *` veri türlerinde taramaları çalıştırmak pahalı olduğundan gerektiğinde. 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Veri türleri gönderen bilgisayarlar / saat sayısı faturalandırılır döndürmek için Genişletilebilir:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="billedsize"></a>\_BilledSize
**\_BilledSize** özelliği, Azure hesabınızda faturalandırılırsınız veri bayt cinsinden boyutunu belirtir  **\_IsBillable** geçerlidir.

### <a name="examples"></a>Örnekler
Bilgisayar başına alınan Faturalanabilir olayların boyutunu görmek için `_BilledSize` bayt cinsinden boyut sağlayan özelliği:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last 
```

Abonelik başına alınan Faturalanabilir olayların boyutunu görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId | sort by Bytes nulls last 
```

Kaynak grubu başına alınan Faturalanabilir olayların boyutunu görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/resourcegroups/" ResourceGroupName "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId, ResourceGroupName | sort by Bytes nulls last 

```


Bilgisayar başına alınan olayların sayısını görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last
```

Bilgisayar başına alınan Faturalanabilir olayların sayısını görmek için aşağıdaki sorguyu kullanın: 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last
```

Belirli bir bilgisayar Faturalanabilir veri türlerinden sayısını görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last 
```

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl hakkında daha fazla bilgiyi [Azure İzleyici günlük verilerinin depolandığı](../log-query/log-query-overview.md).
- Ders almak [günlük sorguları yazma](../../azure-monitor/log-query/get-started-queries.md).
- Ders almak [günlük sorgularda tabloları birleştirme](../../azure-monitor/log-query/joins.md).
