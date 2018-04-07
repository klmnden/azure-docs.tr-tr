---
title: Olay hub'ı alıcılar Azure akış analizi sorunlarını giderme
description: Olay hub'ları tüketici grupları, Stream Analytics işlerini dikkate için en iyi uygulamaları sorgulayın.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: 20614986fc6c6afa9a92d163bf973a148e0517c0
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a>Azure Stream Analytics olay hub'ı alıcıları ile hata ayıklama

Alma veya bir işi veri çıkışı için Azure Stream Analytics Azure Event Hubs kullanabilirsiniz. Olay hub'ları kullanmak için en iyi uygulama iş ölçeklenebilirlik sağlamak için birden çok tüketici grupları, kullanmaktır. Belirli bir giriş için Stream Analytics işinde okuyucu sayısını tek bir tüketici grubundaki okuyucu sayısını etkileyen bir nedenidir. Alıcıları kesin sayısı iç uygulama ayrıntılarını genişleme topoloji mantığı için temel alır. Alıcıların sayısına harici olarak gösterilmez. Okuyucu sayısını iş başlangıç saatinde veya iş yükseltme sırasında değiştirebilirsiniz.

> [!NOTE]
> Okuyucu sayısını iş yükseltme sırasında değiştiğinde geçici uyarıları denetim günlükleri için yazılmıştır. Akış analizi işleri, bu geçici sorunları otomatik olarak kurtarın.

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Bölüm başına okuyucu sayısını beş olay hub'ları sınırını aşıyor

Bölüm başına okuyucu sayısını beş olay hub'ları sınırını aşıyor senaryolar aşağıdakileri içerir:

* Birden çok SELECT deyimine: başvuran birden çok SELECT deyimine kullanırsanız **aynı** olay hub'ı girin, her SELECT deyiminin oluşturulacak yeni bir alıcı neden olur.
* BİRLEŞİM: bir birleşim kullandığınızda, bunu başvuran birden çok girişi olması mümkündür **aynı** olay hub'ı ve tüketici grubu.
* Kendi KENDİNE BİRLEŞİMDE: SELF katılma işlemi kullandığınızda, başvurmak mümkündür **aynı** olay hub'ı birden çok kez.

## <a name="solution"></a>Çözüm

Aşağıdaki en iyi yöntemler, bölüm başına okuyucu sayısını beş olay hub'ları sınırını aşıyor senaryoları azaltılmasına yardımcı olur.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>WITH yan tümcesini kullanarak sorgunuzu içine birden çok adımı bölme

WITH yan tümcesi sorgusunda FROM yan tümcesi tarafından başvurulan bir geçici adlandırılmış sonuç kümesi belirtir. WITH yan tümcesi tek bir SELECT deyimi yürütme kapsamını tanımlayın.

Örneğin, bu sorgu yerine:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Bu sorguyu kullanın:

```
WITH data AS (
   SELECT * FROM inputEventHub
)

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Girişleri farklı tüketici grupları bağlamak emin olun

Üç veya daha fazla girdi aynı olay hub'ları tüketici grubuna bağlı olan sorguları için ayrı tüketici grupları oluşturun. Bu ek Stream Analytics girişleri oluşturulmasını gerektirir.


## <a name="get-help"></a>Yardım alın
Ek Yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
