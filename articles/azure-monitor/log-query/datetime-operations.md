---
title: Azure Log Analytics sorgu tarih saat değerleri ile çalışma | Microsoft Docs
description: Log Analytics sorguları tarih ve saat verilerini ile nasıl çalışılacağını açıklar.
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
ms.openlocfilehash: 15767107a5c535cfda98da2a5177e15ca221f35d
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214703"
---
# <a name="working-with-date-time-values-in-log-analytics-queries"></a>Log Analytics sorgu tarih saat değerleri ile çalışma

> [!NOTE]
> Tamamlamanız gereken [Analytics portalı ile çalışmaya başlama](get-started-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, tarih ve saat verilerini Log Analytics sorgu ile nasıl çalışılacağı açıklanır.


## <a name="date-time-basics"></a>Tarih saat temelleri
Log Analytics sorgu dili, tarihler ve saatler ile ilişkili iki ana veri türü vardır: datetime ve TimeSpan değeri. Tüm tarihler UTC cinsinden ifade edilir. Birden fazla tarih/saat biçimleri desteklenir, ancak ISO8601 biçimini tercih edilir. 

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

Tarih saat dizesi kullanarak atayarak oluşturulabilir `todatetime` işleci. Örneğin, belirli bir zaman çerçevesinde gönderilen VM sinyal gözden geçirmek için yapabileceğiniz kullanım [işleci arasında](/azure/kusto/query/betweenoperator) zaman aralığı belirtmek için uygun olduğu...

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
Bir tarih/saat veya timespan varsayılan dışında bir zaman birimi olarak ifade etmek yararlı olabilir. Örneğin, son 30 dakika hata olayları gözden geçiriyor olmanızdan varsayalım ve ne kadar zaman önce olayın gerçekleştiği gösteren bir hesaplanmış sütun gerekir:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

Gördüğünüz _timeAgo_ sütun değerleri aşağıdaki gibi tutar: "hh:mm:ss.fffffff biçimlendirilmiş anlamı 00:09:31.5118992". Bu değerleri biçimlendirmek istiyorsanız _numver_ başlangıç zamanından itibaren dakika sayısı, bu değer yalnızca "1 dakika artan" bölün.:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Toplamalar ve zaman aralıklarına göre benzeyebilir
Belirli süre boyunca belirli bir zaman dilimi içinde istatistiklerini elde ihtiyacı, başka bir yaygın senaryodur. Bu, bir `bin` işleci summarıze yan tümcesinin bir parçası olarak kullanılabilir.

Son yarım saat boyunca 5 dakikada gerçekleşen olayların sayısını almak için aşağıdaki sorguyu kullanın:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Bu, aşağıdaki tabloda oluşturur:  
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

Bu, aşağıdaki sonuçları oluşturur:

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7,136|
|2018-07-29T00:00:00.000|12,315|
|2018-07-30T00:00:00.000|16,847|
|2018-07-31T00:00:00.000|12,616|
|2018-08-01T00:00:00.000|5,416  |


## <a name="time-zones"></a>Saat dilimleri
Tüm tarih/saat değerleri UTC cinsinden ifade edilir olduğundan, genellikle bu yerel saat dilimi dönüştürmek kullanışlıdır. Örneğin, bu hesaplama için PST saatleri UTC dönüştürmek için kullanın:

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
| Bir tarih değerine göre Al  | [endofday](/azure/kusto/query/endofdayfunction) [endofweek](/azure/kusto/query/endofweekfunction) [endofmonth](/azure/kusto/query/endofmonthfunction) [endofyear](/azure/kusto/query/endofyearfunction) [startofday](/azure/kusto/query/startofdayfunction) [startofweek](/azure/kusto/query/startofweekfunction) [startofmonth](/azure/kusto/query/startofmonthfunction) [startofyear](/azure/kusto/query/startofyearfunction) |

## <a name="next-steps"></a>Sonraki adımlar
Log Analytics sorgu dilini kullanarak için diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)