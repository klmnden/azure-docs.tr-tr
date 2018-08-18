---
title: Azure Log Analytics sorgu dizeleri ile çalışma | Microsoft Docs
description: Bu makale, Log Analytics'te sorgu yazmak için Analytics portalı kullanmaya yönelik bir öğretici sağlar.
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
ms.openlocfilehash: f027754f26a9063aa5faa548fd01576624811005
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190328"
---
# <a name="working-with-json-and-data-structures-in-log-analytics-queries"></a>JSON ve veri yapıları Log Analytics sorguları çalışma

> [!NOTE]
> Tamamlamanız gereken [Analytics portalı ile çalışmaya başlama](get-started-analytics-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.


İç içe nesneler içeren bir dizi veya anahtar-değer çiftlerinin bir eşlem içindeki diğer nesnelerin nesnelerdir. Bu nesneler, JSON dize olarak temsil edilir. Bu makale, JSON verilerini almak ve iç içe geçmiş nesnelerde analiz etmek için nasıl kullanıldığını açıklar.

## <a name="working-with-json-strings"></a>JSON dizeler ile çalışma
Kullanım `extractjson` belirli bir JSON öğesi bilinen bir yolda erişmek için. Bu işlev, aşağıdaki kuralları kullanan bir yol ifadesi gerektirir.

- _$_ kök klasörü belirtmek için
- Aşağıdaki örneklerde gösterildiği gibi dizinleri ve öğeleri başvurmak için köşeli ayraç veya nokta gösterimi kullanın.


Öğeleri ayırmak için dizinler ve nokta için köşeli ayraç kullanın:

```OQL
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report
| extend status = extractjson("$.hosts[0].status", hosts_report)
```

Bu yalnızca köşeli ayraçlar gösterimini kullanarak aynı sonucu.

```OQL
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report 
| extend status = extractjson("$['hosts'][0]['status']", hosts_report)
```

Yalnızca bir öğe yoksa yalnızca nokta gösterimi kullanabilirsiniz:

```OQL
let hosts_report='{"location":"North_DC", "status":"running", "rate":5}';
print hosts_report 
| extend status = hosts_report.status
```


## <a name="working-with-objects"></a>Nesneler ile çalışma

### <a name="parsejson"></a>parsejson
Birden çok öğe, json yapısındaki erişmek için dinamik bir nesne erişmek kolaydır. Kullanım `parsejson` metin verilerine dinamik bir nesne olarak atanamadı. Dinamik bir türe dönüştürülen ek işlevleri ve verileri çözümlemek için kullanılabilir.

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend status0=hosts_object.hosts[0].status, rate1=hosts_object.hosts[1].rate
```



### <a name="arraylength"></a>arraylength
Kullanım `arraylength` bir dizideki öğelerin sayısını:

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend hosts_num=arraylength(hosts_object.hosts)
```

### <a name="mvexpand"></a>mvexpand
Kullanım `mvexpand` bir nesnenin özelliklerini farklı satırlara ayırmak için.

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| mvexpand hosts_object.hosts[0]
```

![mvexpand](media/json-data-structures/mvexpand.png)

### <a name="buildschema"></a>buildschema
Kullanım `buildschema` bir nesnenin tüm değerleri admits şeması get yapılmaya:

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```

Bir şema JSON biçiminde çıktı.
```json
{
    "hosts":
    {
        "indexer":
        {
            "location": "string",
            "rate": "int",
            "status": "string"
        }
    }
}
```
Bu çıkış nesnesi alanları ve eşleşen veri türlerini açıklar. 

İç içe geçmiş nesnelerde aşağıdaki örnekte olduğu gibi farklı şemalar sahip olabilir:

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"status":"stopped", "rate":"3", "range":100}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```


![Şema derleme](media/json-data-structures/buildschema.png)

## <a name="next-steps"></a>Sonraki adımlar
Log Analytics sorgu dilini kullanarak için diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)