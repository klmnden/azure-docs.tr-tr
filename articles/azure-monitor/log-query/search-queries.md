---
title: Azure İzleyici günlüklerine arama sorguları | Microsoft Docs
description: Bu makalede almaya yönelik bir öğretici sağlar. Azure izleyici günlüğü sorgularda arama kullanmaya.
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
ms.date: 08/06/2018
ms.author: bwren
ms.openlocfilehash: b118740f3a57e168c5dfb071c199bcf424bd5113
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295568"
---
# <a name="search-queries-in-azure-monitor-logs"></a>Azure İzleyici günlüklerinde arama sorguları
Azure İzleyici günlük sorguları bir tablo adı veya bir arama komutunu ile başlayabilirsiniz. Bu öğretici, arama tabanlı sorgular kapsar. Her yöntemin avantajları vardır.

Tablo tabanlı sorgular, sorgu kapsamı tarafından başlatın ve bu nedenle arama sorguları daha etkili olma eğilimindedir. Arama sorguları, küçük, daha iyi bir seçenek için belirli bir değer sütunları veya tabloları arasında ararken yaptığına yapılandırılmış ' dir. **Arama** tüm tablolar, belirtilen değere veya bir tablodaki tüm sütunları tarayabilirsiniz. İşlenen veri miktarı bu sorgular daha uzun sürebilir ve çok büyük sonuç kümelerini döndürme olasılığı neden olduğu devasa, olabilir.

## <a name="search-a-term"></a>Arama terimi
**Arama** komutu genellikle belirli bir terim aramak için kullanılır. Aşağıdaki örnekte, tüm tabloların tüm sütunlarında "error" terimi için taranan:

```Kusto
search "error"
| take 100
```

Kullanımı kolay oldukları karşın, yukarıda gösterilen bir gibi kapsamsız sorgular verimli değildir ve birçok ilgisiz sonuçları döndürmek büyük olasılıkla. İlgili tablo veya belirli bir sütuna aramak için daha iyi bir uygulama olacaktır.

### <a name="table-scoping"></a>Tablo kapsamı
Belirli bir tabloda bir terim aramak için ekleme `in (table-name)` hemen sonrasına **arama** işleci:

```Kusto
search in (Event) "error"
| take 100
```

veya birden çok tablo:
```Kusto
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Tablo ve sütun kapsamı
Varsayılan olarak, **arama** veri kümesindeki tüm sütunları değerlendirir. Yalnızca belirli bir sütuna aramak için bu sözdizimini kullanın:

```Kusto
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> Kullanırsanız `==` yerine `:`, sonuçları, kayıtları içerir *kaynak* tam değeri "error" sütununun tam bu durumda. Kullanarak ':' kayıtları içerecek burada *kaynak* "hata kodu 404" veya "Error" gibi değerler vardır.

## <a name="case-sensitivity"></a>Büyük küçük harf duyarlılığı
"Dns" arama "DNS", "dns" veya "Dns" gibi sonuçlar böylece varsayılan olarak, arama terimi, duyarlıdır. Büyük küçük harfe duyarlı arama yapmak için `kind` seçeneği:

```Kusto
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Joker karakter kullanın
**Arama** komutu başlangıcında, son veya döneminin ortasında joker destekler.

"Win" ile başlayan koşulları aramak için:
```Kusto
search in (Event) "win*"
| take 100
```

Aranacak ".com" ile biten koşulları:
```Kusto
search in (Event) "*.com"
| take 100
```

"Www" içeren koşulları aramak için:
```Kusto
search in (Event) "*www*"
| take 100
```

"Corp" ile başlayan ve biten ".com" "corp.mydomain.com" gibi arama terimlerini için"

```Kusto
search in (Event) "corp*.com"
| take 100
```

Ayrıca her şeyi bir tabloda yalnızca bir joker kart kullanarak alabilirsiniz: `search in (Event) *`, ancak yalnızca yazma ile aynı olacaktır `Event`.

> [!TIP]
> Kullanabilirsiniz ancak `search *` her tablonun her sütunu almak için her zaman belirli tablolar için sorgularınızın kapsamını önerilir. Kapsamsız sorgular tamamlanması biraz zaman alabilir ve çok fazla sonuç döndürebilir.

## <a name="add-and--or-to-search-queries"></a>Ekleme *ve* / *veya* aramak için sorgular
Kullanım **ve** birden çok kullanım koşulları içeren kayıtlar aramak için:

```Kusto
search in (Event) "error" and "register"
| take 100
```

Kullanım **veya** koşulları en az birini içeren kayıtları almak için:

```Kusto
search in (Event) "error" or "register"
| take 100
```

Birden çok arama koşulu varsa, aynı sorguyu parantez kullanarak birleştirebilirsiniz:

```Kusto
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

Bu örneğin sonuçlarını "error" terimini içeren ve "Kaydet" veya "Hazırlama" ile başlayan bir şey de içeren kayıtlarının olacaktır.

## <a name="pipe-search-queries"></a>Kanal arama sorguları
Tıpkı diğer herhangi bir komutu olduğu gibi **arama** arama sonuçlarını filtre, sıralar, toplu ve bu nedenle ayrıştırılabilir. Örneğin, sayısını almak için *olay* "win" içeren kayıtlar:

```Kusto
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla ilgili öğreticiler bakın [Kusto sorgu dili site](/azure/kusto/query/).
