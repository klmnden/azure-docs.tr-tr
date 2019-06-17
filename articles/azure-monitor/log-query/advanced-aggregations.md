---
title: Azure İzleyici günlük sorguları toplamalara Gelişmiş | Microsoft Docs
description: Azure İzleyici günlük sorguları için daha gelişmiş toplama seçeneklerini bazılarını açıklar.
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
ms.openlocfilehash: 56e87da0353a41504035a070d4c10bab0dda2279
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60551762"
---
# <a name="advanced-aggregations-in-azure-monitor-log-queries"></a>Azure İzleyici günlük sorguları toplamalara Gelişmiş

> [!NOTE]
> Tamamlamanız gereken [Azure İzleyici sorguları Toplamalara](./aggregations.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, Azure İzleyici sorguları daha gelişmiş toplama seçeneklerini bazılarını açıklar.

## <a name="generating-lists-and-sets"></a>Listeler ve kümeleri oluşturma
Kullanabileceğiniz `makelist` özet verileri belirli bir sütundaki değerleri sıraya göre. Örneğin, en yaygın sipariş olaylar gerçekleştiğinde makinelerinizde incelemek isteyebilirsiniz. Aslında, her makinede Eventıds sıraya göre veri Özet. 

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makelist(EventID) by Computer
```

|Computer|list_EventID|
|---|---|
| Bilgisayar1 | [704,701,1501,1500,1085,704,704,701] |
| bilgisayar2 | [326,105,302,301,300,102] |
| ... | ... |

`makelist` verileri içine geçirilen sırada bir liste oluşturur. Eski olayları için en yeni sıralamak için kullanmak `asc` yerine sipariş deyiminde `desc`. 

Yalnızca benzersiz değerler listesini oluşturmak kullanışlıdır. Bu adlı bir _ayarlamak_ ve ile oluşturulan `makeset`:

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makeset(EventID) by Computer
```

|Computer|list_EventID|
|---|---|
| Bilgisayar1 | [704,701,1501,1500,1085] |
| bilgisayar2 | [326,105,302,301,300,102] |
| ... | ... |

Gibi `makelist`, `makeset` de çalışır sıralı veri ve içine geçirilen Satır bazında tabanlı dizi oluşturur.

## <a name="expanding-lists"></a>Listeleri Genişletiliyor
Ters işleyişini `makelist` veya `makeset` olduğu `mvexpand`, satırları ayırmak için değer listesi genişletir. Dinamik sütun, hem JSON hem de dizi herhangi bir sayıda arasında genişletebilirsiniz. Örneğin, iade edilemedi *sinyal* tablo son bir saat içinde bir sinyal gönderilen bilgisayarlardan veri gönderen çözümleri için:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, Solutions
```

| Computer | Çözümler | 
|--------------|----------------------|
| Bilgisayar1 | "güvenlik", "güncelleştirmeler", "defteriniz" |
| bilgisayar2 | "güvenlik", "güncelleştirmeler" |
| bilgisayar3 | "kötü amaçlı yazılımdan koruma", "defteriniz" |
| ... | ... |

Kullanım `mvexpand` her değeri virgülle ayrılmış bir listesi yerine ayrı bir satırda göstermek için:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
```

| Computer | Çözümler | 
|--------------|----------------------|
| Bilgisayar1 | "güvenlik" |
| Bilgisayar1 | "güncelleştirmeler" |
| Bilgisayar1 | "değişiklik"izleme |
| bilgisayar2 | "güvenlik" |
| bilgisayar2 | "güncelleştirmeler" |
| bilgisayar3 | "kötü amaçlı yazılımdan koruma" |
| bilgisayar3 | "değişiklik"izleme |
| ... | ... |


Ardından kullanabileceğinizi `makelist` yeniden grubuna öğelerini birlikte ve bu süre çözüm başına bilgisayarların listesini bakın:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
| summarize makelist(Computer) by tostring(Solutions) 
```

|Çözümler | list_Computer |
|--------------|----------------------|
| "güvenlik" | ["bilgisayar1", "bilgisayar2"] |
| "güncelleştirmeler" | ["bilgisayar1", "bilgisayar2"] |
| "değişiklik"izleme | ["bilgisayar1", "bilgisayar3"] |
| "kötü amaçlı yazılımdan koruma" | ["bilgisayar3"] |
| ... | ... |

## <a name="handling-missing-bins"></a>Depo eksik işleme
Yararlı bir uygulamayı `mvexpand` eksik depo için varsayılan değerleri doldurmak için gerekli değildir. Örneğin, belirli bir makine için çalışma süresi, sinyal inceleyerek aradığınız varsayalım. Ayrıca, olan sinyal kaynağını görmek istediğiniz _kategori_ sütun. Normalde, biz basit kullanacağınız deyim şu şekilde özetler:

```Kusto
Heartbeat
| where TimeGenerated > ago(12h)
| summarize count() by Category, bin(TimeGenerated, 1h)
```

| Kategori | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Doğrudan aracı | 2017-06-06T17:00:00Z | 15 |
| Doğrudan aracı | 2017-06-06T18:00:00Z | 60 |
| Doğrudan aracı | 2017-06-06T20:00:00Z | 55 |
| Doğrudan aracı | 2017-06-06T21:00:00Z | 57 |
| Doğrudan aracı | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |

Bu ancak ilişkili demetine sonuçları "2017-06-06T19:00:00Z" Bu saat için herhangi bir sinyal veri olmadığından eksik. Kullanım `make-series` boş demet için bir varsayılan değer atamak için işlevi. Bu, her iki ek bir dizi sütun kategorisiyle, değerleri için diğeri için eşleşen zaman demet için bir satır oluşturur:

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
```

| Kategori | count_ | TimeGenerated |
|---|---|---|
| Doğrudan aracı | [15,60,0,55,60,57,60,...] | ["2017-06-06T17:00:00.0000000Z","2017-06-06T18:00:00.0000000Z","2017-06-06T19:00:00.0000000Z","2017-06-06T20:00:00.0000000Z","2017-06-06T21:00:00.0000000Z",...] |
| ... | ... | ... |

Üçüncü öğesine *count_* dizi 0 beklendiği gibi olduğu ve eşleşen bir zaman damgası yok "2017-06-06T19:00:00.0000000Z" içinde _TimeGenerated_ dizisi. Bu dizi biçimi, ancak okuma zordur. Kullanım `mvexpand` diziler genişletin ve aynı biçimi tarafından oluşturulan çıktı üretmek için `summarize`:

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
| mvexpand TimeGenerated, count_
| project Category, TimeGenerated, count_
```

| Kategori | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Doğrudan aracı | 2017-06-06T17:00:00Z | 15 |
| Doğrudan aracı | 2017-06-06T18:00:00Z | 60 |
| Doğrudan aracı | 2017-06-06T19:00:00Z | 0 |
| Doğrudan aracı | 2017-06-06T20:00:00Z | 55 |
| Doğrudan aracı | 2017-06-06T21:00:00Z | 57 |
| Doğrudan aracı | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |



## <a name="narrowing-results-to-a-set-of-elements-let-makeset-toscalar-in"></a>Bir öğe kümesini sonuçları daraltma: `let`, `makeset`, `toscalar`, `in`
Yaygın bir senaryo, bir ölçüt kümesi temel alınarak belirli bazı varlıklar adını seçin ve daha sonra farklı bir veri kümesi, bir dizi varlık aşağı filtre sağlamaktır. Örneğin, güncelleştirmelerin eksik olduğu bilinen bilgisayarlar bulmak ve tanımlamak için bu bilgisayarları çekilerek IP'ler:


```Kusto
let ComputersNeedingUpdate = toscalar(
    Update
    | summarize makeset(Computer)
    | project set_Computer
);
WindowsFirewall
| where Computer in (ComputersNeedingUpdate)
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