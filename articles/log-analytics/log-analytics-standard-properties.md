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
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 7282734b3524d7dfa80c54d074aac2268e38c5ab
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419398"
---
# <a name="standard-properties-in-log-analytics-records"></a>Log Analytics kayıtları standart özellikler
Verileri [Log Analytics](../log-analytics/log-analytics-queries.md) kümesi her bir özellik kümesi olan bir özel veri türü ile kayıt olarak depolanır. Birçok veri türleri, birden çok türlerinde ortak olan standart özellikleri olacaktır. Bu makalede, bu özellikleri açıklar ve nasıl bunları sorgularında kullanabileceğiniz örnekler sağlar.

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

## <a name="resourceid"></a>_ResourceId
**_ResourceId** özelliği, kaydı ile ilişkili kaynak için benzersiz bir tanımlayıcı tutar. Bu, sorgunuzu kayıtlarına yalnızca belirli bir kaynaktan kapsam veya ilgili verileri birden çok tabloda katılmak için kullanılacak bir standart özelliği sağlar.

Değerini, Azure kaynakları için **_ResourceId** olduğu [Azure kaynak kimliği URL](../azure-resource-manager/resource-group-template-functions-resource.md). Şu anda Azure kaynaklarına sınırlı bir özelliğidir, ancak şirket içi bilgisayarlar gibi Azure dışındaki kaynaklar için genişletilir.

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

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl hakkında daha fazla bilgiyi [Log Analytics verilerinin depolandığı](../log-analytics/log-analytics-queries.md).
- Ders almak [Log Analytics'te sorgu yazma](../log-analytics/query-language/get-started-queries.md).
- Ders almak [Log Analytics sorguları tabloları birleştirme](../log-analytics/query-language/joins.md).