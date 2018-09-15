---
title: Azure Log Analytics sorguları toplamalara | Microsoft Docs
description: Verilerinizi analiz etmek için faydalı teklif Log Analytics sorgu toplama işlevleri açıklar.
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
ms.openlocfilehash: f72fb6f654b4699214a22a7f96431c605af52f2d
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603682"
---
# <a name="aggregations-in-log-analytics-queries"></a>Log Analytics sorguları toplamaları

> [!NOTE]
> Tamamlamanız gereken [Analytics portalı ile çalışmaya başlama](get-started-analytics-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, verilerinizi analiz etmek için faydalı teklif Log Analytics sorgu toplama işlevleri açıklanmaktadır. Tüm bu işlevler çalışmak `summarize` işleci girdi tablosunun toplu sonuçları içeren bir tablo oluşturur.

## <a name="counts"></a>Sayılan

### <a name="count"></a>count
Tüm filtreler uygulandıktan sonra sonuç satır sayısı. Aşağıdaki örnek toplam satır sayısı döndürür _Perf_ son 30 dakika tablosundan. Adlı bir sütunu sonuç döndürülmeden *count_* sürece belirli bir ad atayın:


```KQL
Perf
| where TimeGenerated > ago(30m) 
| summarize count()
```

```KQL
Perf
| where TimeGenerated > ago(30m) 
| summarize num_of_records=count() 
```

Bir zaman grafiğini görselleştirme, zaman içinde bir eğilim görmek yararlı olabilir:

```KQL
Perf 
| where TimeGenerated > ago(30m) 
| summarize count() by bin(TimeGenerated, 5m)
| render timechart
```

Bu örnekteki Çıkışta, 5 dakikalık aralıklarla iyileştirilmiş kayıt sayısı eğilim çizgisi gösterir:

![Sayısı Eğilimi](media/aggregations/count-trend.png)


### <a name="dcount-dcountif"></a>DCount, dcountif
Kullanım `dcount` ve `dcountif` belirli bir sütundaki farklı değerleri saymak için. Aşağıdaki sorgu, kaç farklı bilgisayarların sinyal son bir saat içinde gönderilen değerlendirir:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcount(Computer)
```

Sinyal gönderilen Linux bilgisayarları Say kullanın `dcountif`:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcountif(Computer, OSType=="Linux")
```

### <a name="evaluating-subgroups"></a>Alt gruplar değerlendiriliyor
Alt gruplar veri çubuğunda bir sayı veya diğer toplamalar gerçekleştirmek için `by` anahtar sözcüğü. Örneğin, sinyal gönderilen her ülkede ayrı Linux bilgisayarların sayısını saymak için şunu yazın:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry
```

|RemoteIPCountry  | distinct_computers  |
------------------|---------------------|
|Amerika Birleşik Devletleri    | 19                  |
|Kanada           | 3                   |
|İrlanda          | 0                   |
|Birleşik Krallık   | 0                   |
|Hollanda      | 2                   |


Bile küçük alt grupları verilerinizi analiz etmek için ek sütun adlarına ekleme `by` bölümü. Örneğin, farklı bilgisayarların her ülkenin OSType başına saymak isteyebilirsiniz:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry, OSType
```

## <a name="percentiles-and-variance"></a>Yüzdebirliklerini hesapla ve varyans
Sayısal değerleri değerlendirirken, bir ortak alışkanlıktır bunları ortalama kullanarak `summarize avg(expression)`. Ortalamalar, yalnızca birkaç durum niteleyen aşırı değerler tarafından etkilenir. Bu sorunu gidermek için daha az hassas işlevleri gibi kullanabilirsiniz `median` veya `variance`.

### <a name="percentile"></a>Yüzdebirlik
ORTANCA değerini bulmak için kullanmak `percentile` işlevi yüzdelik dilim belirtmek için bir değer ile:

```KQL
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 50) by Computer
```

Ayrıca, her biri için toplu bir sonuç almak için farklı yüzdebirliklerini belirtebilirsiniz:

```KQL
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 25, 50, 75, 90) by Computer
```

Bu bilgisayar bazı CPU'yu benzer ORTANCA değerlere sahip, ancak bazı ORTANCA sürekli olsa da, diğer bilgisayarlar daha düşük ve yüksek CPU değer bunlar ani yaşadı anlamı bildirdiniz gösterebilir.

### <a name="variance"></a>Varyans
Doğrudan bir değerin varyansını değerlendirmek için standart sapma ve varyans yöntemleri kullanın:

```KQL
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), variance(CounterValue) by Computer
```

CPU kullanımı kararlılığını analiz etmek için en iyi yolu ile ORTANCA olan hesaplamayı stdev oluşturmaktır:

```KQL
Perf
| where TimeGenerated > ago(130m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), percentiles(CounterValue, 50) by Computer
```

Log Analytics sorgu dilini kullanarak için diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)
