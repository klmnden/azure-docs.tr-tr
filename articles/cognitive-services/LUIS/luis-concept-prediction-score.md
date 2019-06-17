---
title: Tahmin puanları
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS API'si hizmette olduğunu tahmin sonuçlarını için güvenilirlik derecesi tahmin puanı gösterir bir kullanıcı utterance üzerinde temel.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 383ce4c4248f7e21f745f503c74a29cb613983e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813998"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Tahmin doğruluğunu amaç ve varlıkları için tahmin puanlar

LUIS sahip tahmin sonuçlarını için güvenilirlik derecesi tahmin puanı gösteren bir kullanıcı utterance üzerinde temel.

Tahmin puanı, sıfır (0) ve bir (1) ' dir. Bir yüksek oranda başarılara LUIS puan 0.99 örneğidir. Düşük güvenilirlik puanı 0,01 örneğidir. 

|Puanı değeri|Güven|
|--|--|
|1|kesin eşleştir|
|0.99|yüksek güvenilirlik|
|0.01|düşük güven|
|0|eşleştirilecek kesin hatası|

LUIS, bir utterance içinde bir düşük güvenilirlik puanı sonuçlandığında vurgular [LUIS](luis-reference-regions.md) Web sitesi **hedefi** ile tanımlanan, sayfa **etiketli hedefi** kırmızı özetlenen.

![Puan uyuşmazlık](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Üst Puanlama hedefi

Bir üst Puanlama amacı her utterance tahmin döndürür. Bu tahmin, tahmin puanları bir sayısal karşılaştırma olur. Üst 2 puanları, bunlar arasında çok küçük bir fark olabilir. LUIS, en çok puan döndüren dışında bu yakınlık göstermiyor.  

## <a name="return-prediction-score-for-all-intents"></a>Tüm amaçlar için tahmin puanı döndürür

Bir test veya uç nokta sonuç, tüm hedefleri içerebilir. Bu yapılandırma üzerinde ayarlanmış [uç nokta](https://aka.ms/v1-endpoint-api-docs) ile `verbose=true` sorgu dizesi ad/değer çifti.

## <a name="review-intents-with-similar-scores"></a>Benzer puanları amaçlarıyla gözden geçirin

Tüm hedefleri puanını gözden geçirme, yalnızca doğru amaç tanımlanır ancak sonraki amaç'ın puanı tanımlanan tutarlı bir şekilde konuşma için önemli ölçüde daha düşük olduğunu doğrulamak için iyi bir yoludur.

LUIS, birden çok ıntents Kapat tahmin puanları, bir utterance içeriğine göre varsa amaçları arasında geçiş yapabilirsiniz. Bu durumu düzeltmek için her amaca daha geniş kitlelere bağlamsal farklar çeşitli konuşma eklemeye devam edin veya bir sohbet Robotu gibi bir istemci uygulamanın sahip, 2 üst ıntents nasıl ele alınacağını hakkında programlı seçimler.

Çok-yakından puanlanır 2 amacı nedeniyle belirleyici eğitim ters. En çok puan ikinci üst ve ilk üst puanı ikinci en çok puan haline gelir. Bu durumu önlemek için her üst iki amacı, utterance sözcük seçimi ve 2 ıntents ayırır bağlamı için örnek konuşma ekleyin. İki amacı hakkında örnek konuşma aynı sayıda olmalıdır. Bir eğitim, nedeniyle tersine çevirme önlemek ayırma için udur puanları % 15 fark.

Tarafından belirleyici eğitim kapatabilirsiniz [tüm verilerle eğitim](luis-how-to-train.md#train-with-all-data).

## <a name="differences-with-predictions-between-different-training-sessions"></a>Öngörüler farklı eğitim oturumları arasındaki farklılıklar

Belirleyici eğitim (rastgele bir öğe) olduğundan farklı bir uygulamada aynı modeli eğitmek ve puanlar aynı değildir, bu farktır. İkincisi, birden fazla hedefi için bir utterance herhangi bir çakışma üst hedefi aynı utterance için eğitim göre değiştirebilirsiniz anlamına gelir.

Bir güven göstermek için belirli bir LUIS puanı, sohbet Robotu gerektiriyorsa, üstteki iki amacı puanı birbirinden kullanmanız gerekir. Bu durum, eğitim çeşitleri için esneklik sağlar.

## <a name="e-exponent-notation"></a>E (üstel) gösterimde

Tahmin puanları, üstel gösterim kullanabilir *görünen* 0-1 yukarıda gibi aralığı `9.910309E-07`. Bu puanı göstergesidir bir çok **küçük** sayı.

|E gösterimi puanı |Gerçek puanı|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="punctuation"></a>Noktalama işaretleri

Noktalama, LUIS, ayrı bir belirteçtir. Bir süre sonunda içermeyen bir utterance ve sonunda nokta içeren bir utterance iki ayrı konuşma olan ve iki farklı Öngörüler elde edebilirsiniz. Model emin noktalama işareti ya da işleme içinde [örnek konuşma](luis-concept-utterance.md) (sahip ve noktalama işaretleri olmaması) veya [desenleri](luis-concept-patterns.md) noktalama özel söz dizimi ile yok saymak daha kolay olduğu: `I am applying for the {Job} position[.]`

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.
