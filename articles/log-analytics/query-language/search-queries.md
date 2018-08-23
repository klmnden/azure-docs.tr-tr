---
title: Log Analytics'te Ara sorgular | Microsoft Docs
description: Bu makalede, başlarken bir öğretici sağlar Log Analytics arama sorgu yazma.
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
ms.date: 08/06/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 6a375da3c97790bd6a7a6fa505de82b2fc298385
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42062118"
---
# <a name="search-queries-in-log-analytics"></a>Log Analytics arama sorguları

> [!NOTE]
> Tamamlamanız gereken [sorgular, Log Analytics ile çalışmaya başlama](get-started-queries.md) Bu öğreticiyi tamamlamadan önce.

Azure Log Analytics sorguları bir tablo adı veya bir arama komutunu ile başlayabilirsiniz. Bu öğretici, arama tabanlı sorgular kapsar. Her yöntemin avantajları vardır.

Tablo tabanlı sorgular, sorgu kapsamı tarafından başlatın ve bu nedenle arama sorguları daha etkili olma eğilimindedir. Arama sorguları, küçük, daha iyi bir seçenek için belirli bir değer sütunları veya tabloları arasında ararken yaptığına yapılandırılmış ' dir. **Arama** tüm tablolar, belirtilen değere veya bir tablodaki tüm sütunları tarayabilirsiniz. İşlenen veri miktarı bu sorgular daha uzun sürebilir ve çok büyük sonuç kümelerini döndürme olasılığı neden olduğu devasa, olabilir.

## <a name="search-a-term"></a>Arama terimi
**Arama** komutu genellikle belirli bir terim aramak için kullanılır. Aşağıdaki örnekte, tüm tabloların tüm sütunlarında "error" terimi için taranan:

```OQL
search "error"
| take 100
```

Kullanımı kolay oldukları karşın, yukarıda gösterilen bir gibi kapsamsız sorgular verimli değildir ve birçok ilgisiz sonuçları döndürmek büyük olasılıkla. İlgili tablo veya belirli bir sütuna aramak için daha iyi bir uygulama olacaktır.

### <a name="table-scoping"></a>Tablo kapsamı
Belirli bir tabloda bir terim aramak için ekleme `in (table-name)` hemen sonrasına **arama** işleci:

```OQL
search in (Event) "error"
| take 100
```

veya birden çok tablo:
```OQL
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Tablo ve sütun kapsamı
Varsayılan olarak, **arama** veri kümesindeki tüm sütunları değerlendirir. Yalnızca belirli bir sütuna aramak için bu sözdizimini kullanın:

```OQL
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> Kullanırsanız `==` yerine `:`, sonuçları, kayıtları içerir *kaynak* tam değeri "error" sütununun tam bu durumda. Kullanarak ':' kayıt içermez burada *kaynak* "hata kodu 404" veya "Error" gibi değerler vardır.

## <a name="case-sensitivity"></a>Büyük küçük harf duyarlılığı
"Dns" arama "DNS", "dns" veya "Dns" gibi sonuçlar böylece varsayılan olarak, arama terimi, duyarlıdır. Büyük küçük harfe duyarlı arama yapmak için `kind` seçeneği:

```OQL
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Joker karakter kullanın
**Arama** komutu başlangıcında, son veya döneminin ortasında joker destekler.

"Win" ile başlayan koşulları aramak için:
```OQL
search in (Event) "win*"
| take 100
```

Aranacak ".com" ile biten koşulları:
```OQL
search in (Event) "*.com"
| take 100
```

"Www" içeren koşulları aramak için:
```OQL
search in (Event) "*www*"
| take 100
```

"Corp" ile başlayan ve biten ".com" "corp.mydomain.com" gibi arama terimlerini için"

```OQL
search in (Event) "corp*.com"
| take 100
```

Ayrıca her şeyi bir tabloda yalnızca bir joker kart kullanarak alabilirsiniz: `search in (Event) *`, ancak yalnızca yazma ile aynı olacaktır `Event`.

> [!TIP]
> Kullanabilirsiniz ancak `search *` her tablonun her sütunu almak için her zaman belirli tablolar için sorgularınızın kapsamını önerilir. Kapsamsız sorgular tamamlanması biraz zaman alabilir ve çok fazla sonuç döndürebilir.

## <a name="add-and--or-to-search-queries"></a>Ekleme *ve* / *veya* aramak için sorgular
Kullanım **ve** birden çok kullanım koşulları içeren kayıtlar aramak için:

```OQL
search in (Event) "error" and "register"
| take 100
```

Kullanım **veya** koşulları en az birini içeren kayıtları almak için:

```OQL
search in (Event) "error" or "register"
| take 100
```

Birden çok arama koşulu varsa, aynı sorguyu parantez kullanarak birleştirebilirsiniz:

```OQL
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

Bu örneğin sonuçlarını "error" terimini içeren ve "Kaydet" veya "Hazırlama" ile başlayan bir şey de içeren kayıtlarının olacaktır.

## <a name="pipe-search-queries"></a>Kanal arama sorguları
Tıpkı diğer herhangi bir komutu olduğu gibi **arama** arama sonuçlarını filtre, sıralar, toplu ve bu nedenle ayrıştırılabilir. Örneğin, sayısını almak için *olay* "win" içeren kayıtlar:

```OQL
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla ilgili öğreticiler bakın [Log Analytics sorgu dili site](https://docs.loganalytics.io)