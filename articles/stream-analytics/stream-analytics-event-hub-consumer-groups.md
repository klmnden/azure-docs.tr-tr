---
title: Olay hub'ı alıcılar Azure akış analizi sorunlarını giderme
description: Bu makalede, akış analizi işleri, olay hub'ları girişleri için birden çok tüketici grubu kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: aaa8c4e8d273b44f453d3f63f0be1d4baf980649
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="troubleshoot-event-hub-receivers-in-azure-stream-analytics"></a>Olay hub'ı alıcılar Azure akış analizi sorunlarını giderme

Alma veya bir işi veri çıkışı için Azure Stream Analytics Azure Event Hubs kullanabilirsiniz. Olay hub'ları kullanmak için en iyi uygulama iş ölçeklenebilirlik sağlamak için birden çok tüketici grupları, kullanmaktır. Belirli bir giriş için Stream Analytics işinde okuyucu sayısını tek bir tüketici grubundaki okuyucu sayısını etkileyen bir nedenidir. Alıcıları kesin sayısı iç uygulama ayrıntılarını genişleme topoloji mantığı için temel alır. Alıcıların sayısına harici olarak gösterilmez. Okuyucu sayısını iş başlangıç saatinde veya iş yükseltme sırasında değiştirebilirsiniz.

Alıcıları sayısı en yüksek değeri aştığında gösterilen hata.: `The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Okuyucu sayısını iş yükseltme sırasında değiştiğinde geçici uyarıları denetim günlükleri için yazılmıştır. Akış analizi işleri, bu geçici sorunları otomatik olarak kurtarın.

## <a name="add-a-consumer-group-in-event-hubs"></a>Olay hub ' bir tüketici grubu Ekle
Olay hub'ları örneğinizi yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın.

2. Olay hub'larınız bulun.

3. Seçin **Event Hubs** altında **varlıklar** başlığı.

4. Ada göre olay hub'ı seçin.

5. Üzerinde **olay hub'ları örneği** sayfasında **varlıklar** başlığını seçin **tüketici grupları**. Ada sahip bir tüketici grubu **$Default** listelenir.

6. Seçin **+ tüketici grubu** yeni bir tüketici grubu eklemek için. 

   ![Olay hub ' bir tüketici grubu Ekle](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Olay Hub'ına işaret edecek şekilde Stream Analytics işinde giriş oluşturduğunuzda tüketici grubu belirttiniz. Belirtilmediğinde $Default kullanılır. Yeni bir tüketici grubu oluşturduktan sonra Stream Analytics işi olay hub'ı giriş düzenleyin ve yeni tüketici grubu adını belirtin.


## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Bölüm başına okuyucu sayısını beş olay hub'ları sınırını aşıyor

Akış sorgunuzun sözdiziminin birden çok kez aynı giriş olay hub'ı kaynağı başvuruyorsa, iş altyapısı aynı tüketici grubu sorgudan başına birden çok okuyucular kullanabilirsiniz. Aynı tüketici grubu için çok fazla başvuru olduğunda iş beş sınırını aşabilir ve bir hata oluşturulur. Bu durumlarda, aşağıdaki bölümde açıklanan çözümünü kullanarak birden çok tüketici grupları arasında birden çok girişi kullanarak daha fazla bölebilirsiniz. 

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


## <a name="next-steps"></a>Sonraki adımlar
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
