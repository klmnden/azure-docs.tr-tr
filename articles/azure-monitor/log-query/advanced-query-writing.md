---
title: Gelişmiş Azure İzleyicisi'nde sorgular | Microsoft Docs
description: Bu makalede, Azure İzleyicisi'nde sorgular yazmak için Analytics portalı kullanmaya yönelik bir öğretici sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: bwren
ms.openlocfilehash: 65713ed9c2d0635e776a7a7e5f205b6d55438ed4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60589584"
---
# <a name="writing-advanced-queries-in-azure-monitor"></a>Gelişmiş sorgular Azure İzleyici'de yazma

> [!NOTE]
> Tamamlamanız gereken [Azure İzleyici Log Analytics ile çalışmaya başlama](get-started-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

## <a name="reusing-code-with-let"></a>Let koduyla yeniden kullanma
Kullanım `let` sonuçları bir değişkene atayın ve sorgu daha sonra başvurmak için:

```Kusto
// get all events that have level 2 (indicates warning level)
let warning_events=
Event
| where EventLevel == 2;
// count the number of warning events per computer
warning_events
| summarize count() by Computer 
```

Ayrıca, değişkenler sabit değerler atayabilirsiniz. Bu, her sorgu çalıştırmanızda değiştirmek için gereken alanları parametrelerini ayarlamak için bir yöntem destekler. Bu parametre gerektiği gibi değiştirin. Örneğin, boş disk alanı ve boş bellek (yüzdebirliklerini), belirli bir zaman penceresinde hesaplamak için şunu yazın:

```Kusto
let startDate = datetime(2018-08-01T12:55:02);
let endDate = datetime(2018-08-02T13:21:35);
let FreeDiskSpace =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Logical Disk" and CounterName=="Free Megabytes"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
let FreeMemory =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Memory" and CounterName=="Available MBytes Memory"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
union FreeDiskSpace, FreeMemory
```

Bu sorguyu daha sonra çalıştırdığınızda bitiş saati başlangıç değiştirmek kolaylaştırır.

### <a name="local-functions-and-parameters"></a>Yerel işlevler ve parametreleri
Kullanım `let` deyimlerini aynı sorguda kullanılan işlevler oluşturun. Örneğin, bir datetime alanı (UTC biçimi) alan ve standart bir ABD biçimine dönüştürür bir fonksiyon tanımlayın. 

```Kusto
let utc_to_us_date_format = (t:datetime)
{
  strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
  bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
};
Event 
| where TimeGenerated > ago(1h) 
| extend USTimeGenerated = utc_to_us_date_format(TimeGenerated)
| project TimeGenerated, USTimeGenerated, Source, Computer, EventLevel, EventData 
```

## <a name="print"></a>Yazdırma
`print` tek bir sütun ve hesaplama sonucunu gösteren tek bir satır içeren bir tablo döndürür. Bu genellikle, basit bir hesaplama gereken durumlarda kullanılır. Örneğin, Pasifik saati geçerli zamanı bulup EST sahip bir sütun ekleyin:

```Kusto
print nowPst = now()-8h
| extend nowEst = nowPst+3h
```

## <a name="datatable"></a>DataTable
`datatable` bir veri kümesini tanımlamanızı sağlar. Ardından tabloda herhangi bir sorgu öğeleri eklemek kanal oluşturarak ve bir şeması ve bir değerler kümesi sağlar. Örneğin RAM kullanımını bir tablo oluşturun ve saatlik, ortalama değerini hesaplamak için:

```Kusto
datatable (TimeGenerated: datetime, usage_percent: double)
[
  "2018-06-02T15:15:46.3418323Z", 15.5,
  "2018-06-02T15:45:43.1561235Z", 20.2,
  "2018-06-02T16:16:49.2354895Z", 17.3,
  "2018-06-02T16:46:44.9813459Z", 45.7,
  "2018-06-02T17:15:41.7895423Z", 10.9,
  "2018-06-02T17:44:23.9813459Z", 24.7,
  "2018-06-02T18:14:59.7283023Z", 22.3,
  "2018-06-02T18:45:12.1895483Z", 25.4
]
| summarize avg(usage_percent) by bin(TimeGenerated, 1h)
```

DataTable yapıları ayrıca arama tablosu oluştururken çok yararlıdır. Örneğin, olay kimlikleri gibi tablo verilerini eşlemek için _SecurityEvent_ kullanarak olay türleri ile arama tablosu oluşturma, tablo, olay türü için başka bir yerde listelenen `datatable` ve bu datatable katılın  _SecurityEvent_ veri:

```Kusto
let eventCodes = datatable (EventID: int, EventType:string)
[
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4672, "Access",
    4624, "Account activity",
    4799, "Access management",
    4798, "Access management",
    5059, "Key operation",
    4648, "A logon was attempted using explicit credentials",
    4768, "Access management",
    4662, "Other",
    8002, "Process action",
    4904, "Security event management",
    4905, "Security event management",
];
SecurityEvent
| where TimeGenerated > ago(1h) 
| join kind=leftouter (
  eventCodes
) on EventID
| project TimeGenerated, Account, AccountType, Computer, EventType
```

## <a name="next-steps"></a>Sonraki adımlar
Diğer dersler kullanmak için bkz. [Kusto sorgu dili](/azure/kusto/query/) Azure İzleyici ile günlük verilerini:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)
