---
title: Anomali algılayıcısı API kullanırken en iyi yöntemler
description: En iyi yöntemler hakkında Anomali algılayıcısı API'siyle anomalileri tespit edilirken öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 467ac4e475a1c23e25b62c76cfbc959e7ed49465
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484051"
---
# <a name="best-practices-for-using-the-anomaly-detector-api"></a>Anomali algılayıcısı API kullanımı için en iyi uygulamalar

Anomali algılayıcısı API'si, bir durum bilgisi olmayan anomali algılama hizmetidir. Doğruluk ve performans sonuçlarını, tarafından etkilenebilir:

* Zaman serisi verilerinin nasıl hazırlanır.
* Kullanılan Anomali algılayıcısı API parametreler.
* Veri noktası, API isteği sayısı. 

Verileriniz için en iyi sonuçları elde API kullanımı için en iyi uygulamalar hakkında bilgi edinmek için bu makaleyi kullanın. 

## <a name="data-preparation"></a>Veri hazırlama

Zaman serisi Anomali algılayıcısı API kabul biçimlendirilmiş JSON istek nesnesine verileri. Zaman serisi sırayla zaman içinde kayıtlı olan herhangi bir sayısal veri olabilir. Zaman serisi verilerinin windows API'SİNİN performansını artırmak için Anomali algılayıcısı API uç noktası için gönderebilirsiniz. En az sayıda veri noktası gönderebilirsiniz 12, ve en fazla 8640 noktaları. 

Veri noktaları Anomali algılayıcısı API'ye gönderilen geçerli Eşgüdümlü Evrensel Saat (UTC) zaman damgası ve sayısal değer olmalıdır. 

```json
{
    "granularity": "daily",
    "series": [
      {
        "timestamp": "2018-03-01T00:00:00Z",
        "value": 32858923
      },
      {
        "timestamp": "2018-03-02T00:00:00Z",
        "value": 29615278
      },
    ]
}
```

### <a name="missing-data-points"></a>Veri noktaları

Veri noktaları eşit olarak dağıtılmış bir zaman serisi veri kümesi, ince bir ayrıntı düzeyi (küçük bir örnekleme aralığı. özellikle ettiklerinizi ortak For example, veri birkaç dakikada bir örneklenir). Daha az % 10 beklenen veri noktası sayısı eksik algılama sonuçlarınız üzerinde olumsuz bir etkiye sahip olmamalıdır. Özelliklerini veri noktalarından bir önceki süre, doğrusal enterpolasyon veya hareketli ortalama değiştirme gibi temel veri boşlukları doldurma göz önünde bulundurun.

### <a name="aggregate-distributed-data"></a>Dağıtılmış veri toplama

Anomali algılayıcısı API bir eşit olarak dağıtılmış zaman serisinde bulunan en iyi şekilde çalışır. Verilerinizi rastgele dağıtılırsa, bu dakikalık, saatlik veya günlük gibi süre birimi örneğin toplama ölçütü.

## <a name="anomaly-detection-on-data-with-seasonal-patterns"></a>Dönemsel desenlerine sahip veriler üzerinde anomali algılama

Zaman serisi verilerinizle dönemsel desen (bir düzenli aralıklarla gerçekleşir) olduğunu biliyorsanız, doğruluk ve API yanıt süresini artırabilir. 

Belirten bir `period` oluşturduğunuzda, JSON isteğiniz kadar % 50'ye anomali algılama gecikmeyi azaltabilir. `period` Desen yinelemek için kaç kabaca veri noktaları zaman serisi belirten bir tamsayı gerçekleştirir. Örneğin, bir zaman serisi ile günlük bir veri noktası gerekir bir `period` olarak `7`, ve bir zaman serisi (ile aynı haftalık deseni) saatte bir nokta ile olması gereken bir `period` , `7*24`. Veri desenlerini emin değilseniz, bu parametre belirtmeniz gerekmez.

En iyi sonuçlar için 4 sağlamak `period`kullanıcının veri noktası yanı sıra, ek bir değerinde. Örneğin, haftalık desenle saatlik veri yukarıda açıklandığı gibi istek gövdesindeki 673 veri noktası sağlamanız gerekir (`7 * 24 * 4 + 1`).

### <a name="sampling-data-for-real-time-monitoring"></a>Gerçek zamanlı izleme için örnekleme verileri

Akış verilerinizi bir kısa aralıklarla (örneğin, saniyeler veya dakikalar içinde) örneklenir, önerilen veri noktalarının sayısını gönderme Anomali algılayıcısı API'nin en fazla izin verilen (8640 veri noktası sayısıdır) aşabilir. Verilerinizi bir kararlı dönemsel deseni gösteriyorsa, saat gibi daha büyük bir zaman aralığı, zaman serisi verilerinin bir örnek gönderme göz önünde bulundurun. Bu şekilde, verilerinizi örnekleme, API yanıt süresi de fark edilir derecede artırabilir. 

## <a name="next-steps"></a>Sonraki Adımlar

* [Anomali algılayıcısı API nedir?](../overview.md)
* [Hızlı Başlangıç: Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anomalileri algılayın](../quickstarts/detect-data-anomalies-csharp.md)
