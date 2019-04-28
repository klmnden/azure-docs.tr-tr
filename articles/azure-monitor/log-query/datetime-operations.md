---
title: Azure İzleyici günlük sorguları tarih saat değerleri ile çalışma | Microsoft Docs
description: Azure İzleyici günlük sorguları tarih ve saat verilerini ile nasıl çalışılacağını açıklar.
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
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 402511ba3c45e8bd12cb7f92ecd54f6084c8ada2
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62112366"
---
# <a name="working-with-date-time-values-in-azure-monitor-log-queries"></a>Azure İzleyici günlük sorguları tarih saat değerleri ile çalışma

> [!NOTE]
> Tamamlamanız gereken [Analytics portalı ile çalışmaya başlama](get-started-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, Azure İzleyici günlük sorguları tarih ve saat verilerini ile nasıl çalışılacağı açıklanır.


## <a name="date-time-basics"></a>Tarih saat temelleri
Kusto sorgu dili, tarihler ve saatler ile ilişkili iki ana veri türü vardır: datetime ve TimeSpan değeri. Tüm tarihler UTC cinsinden ifade edilir. Birden fazla tarih/saat biçimleri desteklenir, ancak ISO8601 biçimini tercih edilir. 

Timespans zaman birimi tarafından izlenen bir ondalık sayı olarak ifade edilir:

|Toplu özellik   | zaman birimi    |
|:---|:---|
|g           | gün          |
|h           | saat         |
|m           | dakika       |
|s           | second       |
|ms          | Milisaniye  |
|mikrosaniye ölçeğinde | mikrosaniye ölçeğinde  |
|değer çizgisi        | içerir   |

Tarih saat dizesi kullanarak atayarak oluşturulabilir `todatetime` işleci. Örneğin, belirli bir zaman çerçevesinde gönderilen VM sinyal gözden geçirmek için kullanın `between` işlecini bir zaman aralığı belirtin.

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Başka bir yaygın bir senaryo için geçerli bir datetime karşılaştırıyor. Örneğin, son iki dakika boyunca tüm sinyaller görmek için kullanabileceğiniz `now` işleci iki dakika temsil eden bir timespan birlikte:

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

Kısayol, bu işlev için de kullanılabilir:
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

Kısa ve en okunabilir yöntemi ancak kullanarak `ago` işleci:
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

Başlangıç ve bitiş zamanı bilerek yerine, başlangıç saatini ve süresini bildiğiniz varsayalım. Sorguyu şu şekilde yeniden yazabilirsiniz:

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Zaman birimi dönüştürme
Bir tarih/saat veya timespan varsayılan dışında bir saat birimi içinde express isteyebilirsiniz. Örneğin, ne kadar zaman önce gösteren, son 30 dakika hata olayları gözden geçiriyor olmanız ve hesaplanmış bir sütun gerekiyorsa olayın gerçekleştiği:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

`timeAgo` Sütun değerleri aşağıdaki gibi tutar: "hh:mm:ss.fffffff biçimlendirildikten anlamı 00:09:31.5118992". Bu değerleri biçimlendirmek istiyorsanız `numver` başlangıç zamanından itibaren dakika sayısı bu değeri "1 dakika artan" ayırın:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Toplamalar ve zaman aralıklarına göre benzeyebilir
Başka bir yaygın bir senaryo belirli süre boyunca belirli bir zaman dilimi içinde istatistikleri elde etmek için gerekli değildir. Bu senaryo için bir `bin` işleci summarıze yan tümcesinin bir parçası olarak kullanılabilir.

Son yarım saat boyunca 5 dakikada gerçekleşen olayların sayısını almak için aşağıdaki sorguyu kullanın:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Aşağıdaki tablo bu sorgu üretir:  

|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Demetler sonuçları oluşturmak için başka bir işlevler gibi kullanmaktır `startofday`:

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Bu sorgu, aşağıdaki sonuçları üretir:

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7,136|
|2018-07-29T00:00:00.000|12,315|
|2018-07-30T00:00:00.000|16,847|
|2018-07-31T00:00:00.000|12,616|
|2018-08-01T00:00:00.000|5,416|


## <a name="time-zones"></a>Saat dilimleri
Tüm tarih/saat değerleri UTC cinsinden ifade edilir olduğundan, genellikle bu değerlerini yerel saat dilimi dönüştürmek kullanışlıdır. Örneğin, bu hesaplama için PST saatleri UTC dönüştürmek için kullanın:

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>İlgili işlevler

| Kategori | İşlev |
|:---|:---|
| Veri türlerini dönüştürme | [ToDateTime](/azure/kusto/query/todatetimefunction)[totimespan](/azure/kusto/query/totimespanfunction)  |
| Yuvarlatılmış değerine gruplama boyutu | [Depo](/azure/kusto/query/binfunction) |
| Belirli bir tarih veya saat Al | [önce](/azure/kusto/query/agofunction) [artık](/azure/kusto/query/nowfunction)   |
| Değer bir parçası Al | [datetime_part](/azure/kusto/query/datetime-partfunction) [getmonth](/azure/kusto/query/getmonthfunction) [Yılın ayı](/azure/kusto/query/monthofyearfunction) [getyear](/azure/kusto/query/getyearfunction) [dayofmonth](/azure/kusto/query/dayofmonthfunction) [dayofweek](/azure/kusto/query/dayofweekfunction) [dayofyear](/azure/kusto/query/dayofyearfunction) [weekofyear](/azure/kusto/query/weekofyearfunction) |
| Göreli tarih değer alma  | [endofday](/azure/kusto/query/endofdayfunction) [endofweek](/azure/kusto/query/endofweekfunction) [endofmonth](/azure/kusto/query/endofmonthfunction) [endofyear](/azure/kusto/query/endofyearfunction) [startofday](/azure/kusto/query/startofdayfunction) [startofweek](/azure/kusto/query/startofweekfunction) [startofmonth](/azure/kusto/query/startofmonthfunction) [startofyear](/azure/kusto/query/startofyearfunction) |

## <a name="next-steps"></a>Sonraki adımlar
Diğer dersler kullanmak için bkz. [Kusto sorgu dili](/azure/kusto/query/) Azure İzleyici ile günlük verilerini:

- [Dize işlemleri](string-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)