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
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 3a0e2b78de8cea3929ac457bab3d5e07a2b85401
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603388"
---
# <a name="working-with-date-time-values-in-log-analytics-queries"></a>Log Analytics sorgu tarih saat değerleri ile çalışma

> [!NOTE]
> Tamamlamanız gereken [Analytics portalı ile çalışmaya başlama](get-started-analytics-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

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

Tarih saat dizesi kullanarak atayarak oluşturulabilir `todatetime` işleci. Örneğin, belirli bir zaman çerçevesinde gönderilen VM sinyal gözden geçirmek için yapabileceğiniz kullanım [işleci arasında](https://docs.loganalytics.io/docs/Language-Reference/Scalar-operators/between-operator) zaman aralığı belirtmek için uygun olduğu...

```KQL
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Başka bir yaygın bir senaryo için geçerli bir datetime karşılaştırıyor. Örneğin, son iki dakika boyunca tüm sinyaller görmek için kullanabileceğiniz `now` işleci iki dakika temsil eden bir timespan birlikte:

```KQL
Heartbeat
| where TimeGenerated > now() - 2m
```

Kısayol, bu işlev için de kullanılabilir:
```KQL
Heartbeat
| where TimeGenerated > now(-2m)
```

Kısa ve en okunabilir yöntemi ancak kullanarak `ago` işleci:
```KQL
Heartbeat
| where TimeGenerated > ago(2m)
```

Başlangıç ve bitiş zamanı bilerek yerine, başlangıç saatini ve süresini bildiğiniz varsayalım. Sorguyu şu şekilde yeniden yazabilirsiniz:

```KQL
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Zaman birimi dönüştürme
Bir tarih/saat veya timespan varsayılan dışında bir zaman birimi olarak ifade etmek yararlı olabilir. Örneğin, son 30 dakika hata olayları gözden geçiriyor olmanızdan varsayalım ve ne kadar zaman önce olayın gerçekleştiği gösteren bir hesaplanmış sütun gerekir:

```KQL
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

Gördüğünüz _timeAgo_ sütun değerleri aşağıdaki gibi tutar: "00:09:31.5118992", yani bunlar hh:mm:ss.fffffff biçimlendirilir. Bu değerleri biçimlendirmek istiyorsanız _numver_ başlangıç zamanından itibaren dakika sayısı, bu değer yalnızca "1 dakika artan" bölün.:

```KQL
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Toplamalar ve zaman aralıklarına göre benzeyebilir
Belirli süre boyunca belirli bir zaman dilimi içinde istatistiklerini elde ihtiyacı, başka bir yaygın senaryodur. Bu, bir `bin` işleci summarıze yan tümcesinin bir parçası olarak kullanılabilir.

Son yarım saat boyunca 5 dakikada gerçekleşen olayların sayısını almak için aşağıdaki sorguyu kullanın:

```KQL
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

```KQL
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

```KQL
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>İlgili işlevler

| Kategori | İşlev |
|:---|:---|
| Veri türlerini dönüştürme | [ToDateTime](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/todatetime())[totimespan](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/totimespan())  |
| Yuvarlatılmış değerine gruplama boyutu | [Depo](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/bin()) |
| Belirli bir tarih veya saat Al | [önce](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago()) [artık](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/now())   |
| Değer bir parçası Al | [datetime_part](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/datetime_part()) [getmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getmonth()) [Yılın ayı](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/monthofyear()) [getyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getyear()) [dayofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofmonth()) [dayofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofweek()) [dayofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofyear()) [weekofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/weekofyear()) |
| Bir tarih değerine göre Al  | [endofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofday()) [endofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofweek()) [endofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofmonth()) [endofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofyear()) [startofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofday()) [startofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofweek()) [startofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofmonth()) [startofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofyear()) |

## <a name="next-steps"></a>Sonraki adımlar
Log Analytics sorgu dilini kullanarak için diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)