---
title: Azure İzleyici'de verimli günlük sorguları yazma | Microsoft Docs
description: Log Analytics'te sorgu yazmayı öğrenmek için kaynaklara başvurular.
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
ms.date: 01/17/2019
ms.author: bwren
ms.openlocfilehash: 25d6b582ed4d4e24df3841f4191471296e25abd8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60519378"
---
# <a name="writing-efficient-log-queries-in-azure-monitor"></a>Azure İzleyici'de verimli günlük sorguları yazma
Bu makalede, Azure İzleyici'de verimli günlük sorguları yazma ile ilgili öneriler sağlar. Bu stratejiler kullanarak, sorgularınızı hızlı bir şekilde çalışır ve ile en az overheard emin olabilirsiniz.

## <a name="scope-your-query"></a>Sorgunuzun kapsamı
Sorgunuzu gerçekten ihtiyacınız olandan daha fazla veri işleme sahip bir uzun süre çalışan sorgu için sağlama ve çok fazla veri etkili bir şekilde analiz etmek için sonucu genellikle sonuçlanır. Aşırı durumlarda sorgu zaman aşımı çift ve başarısız.

### <a name="specify-your-data-source"></a>Veri kaynağınızı belirtin
Verimli bir sorgu yazılmasını ilk adımı, gerekli veri kaynaklarına kapsamını sınırlayan. Belirten bir tablo, her zaman tercih edilen bir geniş metin araması gibi çalıştıran `search *`. Belirli bir tabloyu sorgulamak üzere sorgunuzu tablo adı olduğu aşağıdaki gibi başlangıcı:

``` Kusto
requests | ...
```

Kullanabileceğiniz [arama](/azure/kusto/query/searchoperator) birden çok sütunda aşağıdaki gibi bir sorgu kullanarak belirli tablolar arasında bir değeri aramak için:

``` Kusto
search in (exceptions) "The server was not found"

search in (exceptions, customEvents) "timeout"
```

Kullanım [birleşim](/azure/kusto/query/unionoperator) aşağıdaki gibi birçok tabloları sorgulamak için:

``` Kusto
union requests, traces | ...
```

### <a name="specify-a-time-range"></a>Bir zaman aralığı belirtin
Ayrıca, ihtiyaç duyduğunuz verileri zaman aralığını sorgunuza sınırlamanız gerekir. Varsayılan olarak, sorgunuzu son 24 saat içindeki toplanan veriler içerir. Bu seçeneği değiştirebilir [zaman aralığı seçicisinin](get-started-portal.md#select-a-time-range) veya açıkça sorgunuza ekleyin. Böylece sorgunuzun rest yalnızca bu aralık içinde veri işleme süresi filtre hemen sonra tablo adını eklemek idealdir:

``` Kusto
requests | where timestamp > ago(1h)

requests | where timestamp between (ago(1h) .. ago(30m))
```
   
### <a name="get-only-the-latest-records"></a>Yalnızca en son kayıtları Al

Yalnızca en son kayıtları döndürmek için *üst* en son 10 kayıtları döndüren şu sorguyu olduğu gibi işleç oturum *izlemeleri* tablosu:

``` Kusto
traces | top 10 by timestamp
```

   
### <a name="filter-records"></a>Kayıtları Filtrele
Belirli bir koşul ile eşleşen günlüklerini gözden geçirmek için *burada* işleci yalnızca kayıtlar döndüren şu sorguyu olduğu gibi _Err_ değeri 0'dan:

``` Kusto
traces | where severityLevel > 0
```



## <a name="string-comparisons"></a>Dize karşılaştırmaları
Zaman [dizeleri değerlendirme](/azure/kusto/query/datatypes-string-operators), genellikle kullanması gereken `has` yerine `contains` tam belirteçleri için ararken. `has` alt dize için arama olmadığı daha verimli olur.

## <a name="returned-columns"></a>Geri dönen sütunlar

Kullanım [proje](/azure/kusto/query/projectoperator) yalnızca için gereksinim duyduğunuz işlenmekte olan sütun kümesini daraltmak için:

``` Kusto
traces 
| project timestamp, severityLevel, client_City 
| ...
```

Kullanabilirsiniz ancak [genişletmek](/azure/kusto/query/extendoperator) değerleri hesaplamak ve kendi sütunları oluşturmak için bunu tipik olarak bir tablo sütununda filtrelemek için daha verimli olacaktır.

Üzerindeki olaylarla filtreler, örneğin, ilk aşağıdaki sorguyu _işlemi\_adı_ oluşturan yeni bir ikinciden daha verimli olacaktır _abonelik_ sütunu ve bu filtreler:

``` Kusto
customEvents 
| where split(operation_Name, "/")[2] == "acb"

customEvents 
| extend subscription = split(operation_Name, "/")[2] 
| where subscription == "acb"
```

## <a name="using-joins"></a>Birleşimleri kullanma
Kullanırken [birleştirme](/azure/kusto/query/joinoperator) işleci, sorgu sol tarafında olması daha az satır içeren tabloyu seçin.


## <a name="next-steps"></a>Sonraki adımlar
Sorgu en iyi uygulamalar hakkında daha fazla bilgi için bkz: [sorgu en iyi](/azure/kusto/query/best-practices).