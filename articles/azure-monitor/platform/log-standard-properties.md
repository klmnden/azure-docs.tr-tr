---
title: Azure İzleyici Log Analytics kayıtları standart özelliklerinde | Microsoft Docs
description: Azure İzleyici Log analytics'te birden çok veri türü için ortak olan özellikleri açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/14/2019
ms.author: bwren
ms.openlocfilehash: abcf3100dc5252db9e3a5e7b446417333a9b37ca
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321900"
---
# <a name="standard-properties-in-log-analytics-records"></a>Log Analytics kayıtları standart özellikler
Verileri [Log Analytics](../log-query/log-query-overview.md) kümesi her bir özellik kümesi olan bir özel veri türü ile kayıt olarak depolanır. Birçok veri türleri, birden çok türlerinde ortak olan standart özellikleri olacaktır. Bu makalede, bu özellikleri açıklar ve nasıl bunları sorgularında kullanabileceğiniz örnekler sağlar.

Bazı veri türleri, ancak henüz diğerleri bunları görebilirsiniz bu özelliklerin bazıları hala uygulanan sürecinde, olduğundan.

## <a name="timegenerated"></a>TimeGenerated
**TimeGenerated** kaydın oluşturulduğu saat ve tarihi özelliği içerir. Bu, filtreleme veya zamana göre özetlemek için kullanılacak ortak bir özellik sağlar. Azure portalında bir zaman aralığı için bir görünüm veya Panoda seçtiğinizde TimeGenerated sonuçları filtrelemek için kullanır.

### <a name="examples"></a>Örnekler

Aşağıdaki sorgu, önceki haftanın her günü için oluşturulan hata olay sayısını döndürür.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

## <a name="type"></a>Tür
**Türü** özellik adını tutan tablosu kayda alındığı, ayrıca, kayıt türü olarak düşünülebilir. Bu özellik, kayıtları kullananlar gibi birden çok tablodan birleştirmek sorgularda yararlıdır `search` farklı türlerde kayıtlar arasında ayrım yapmak için işleci. **$table** yerine kullanılan **türü** bazı yerlerde.

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

## <a name="isbillable"></a>\_IsBillable
 **\_IsBillable** özelliği, içe alınan veri Faturalanabilir olup olmadığını belirtir. Verilerle  **\_IsBillable** eşit _false_ ücretsiz toplanan ve Azure hesabınızda faturalandırılmaz.

### <a name="examples"></a>Örnekler
Faturalandırılan veri türleri gönderme bilgisayarların listesini almak için aşağıdaki sorguyu kullanın:

> [!NOTE]
> İle sorguları kullanma `union withsource = tt *` yürütmek veri veri türlerinde taramalar pahalıdır gerektiğinde. 

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

Sayıları Faturalandırılabilir veri türleri için belirli bir bilgisayar için veri gönderdiğini görmek istiyorsanız, aşağıdaki sorguyu kullanın:

```Kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last 
```


## <a name="next-steps"></a>Sonraki adımlar

- Nasıl hakkında daha fazla bilgiyi [Log Analytics verilerinin depolandığı](../log-query/log-query-overview.md).
- Ders almak [Log Analytics'te sorgu yazma](../../azure-monitor/log-query/get-started-queries.md).
- Ders almak [Log Analytics sorguları tabloları birleştirme](../../azure-monitor/log-query/joins.md).
