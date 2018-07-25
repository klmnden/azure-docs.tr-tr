---
title: LUIS - Azure tarafından döndürülen tahmin puanı anlama | Microsoft Docs
description: LUIS tahmin puanı anlamı öğrenin
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: cee7243531857f07dec2e968352ffb54aef16bf1
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224595"
---
# <a name="prediction-score"></a>Tahmin puanı
Tahmin puanı LUIS sahip tahmin sonuçlarını için güvenilirlik derecesi gösterir. 

Tahmin puan genellikle sıfır (0) ve bir (1) olur. Bir yüksek oranda başarılara LUIS puan 0.99 örneğidir. Düşük güvenilirlik puanı 0,01 örneğidir. 

|Puanı değeri|Güven|
|--|--|
|1|kesin eşleştir|
|0.99|yüksek güvenilirlik|
|0.01|düşük güven|
|0|eşleştirilecek kesin hatası|

LUIS, bir utterance içinde bir düşük güvenilirlik puanı sonuçlandığında vurgular [LUIS](luis-reference-regions.md) Web sitesi **hedefi** ile tanımlanan, sayfa **etiketli hedefi** kırmızı özetlenen. 

![Puan uyuşmazlık](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Üst Puanlama hedefi
Bir üst Puanlama amacı her utterance tahmin döndürür. Tahmin puanları bir sayısal karşılaştırma budur. Üstteki iki puanları, bunlar arasında çok küçük bir fark olabilir. LUIS puanları döndüren dışında bu yakınlık göstermiyor.  

En çok puan yakınlık endişeleriniz varsa, tüm hedefleri puanını döndürmelidir. Sözcük seçimi ile bunların farkını gösteren iki amaçları için konuşma ekleyebilir ve düzenleme veya bir sohbet Robotu gibi LUIS arama uygulamanız varsa, iki üst ıntents nasıl ele alınacağını hakkında programlı seçimler. 

## <a name="return-prediction-score-for-all-intents"></a>Tüm amaçlar için tahmin puanı döndürür
Bir test veya uç nokta sonuç, tüm hedefleri içerebilir. Bu yapılandırma üzerinde ayarlanmış [uç nokta](https://aka.ms/v1-endpoint-api-docs) ile `verbose=true` sorgu dizesi ad/değer çifti. 

## <a name="review-intents-with-similar-scores"></a>Benzer puanları amaçlarıyla gözden geçirin
Tüm hedefleri puanını gözden geçirme, yalnızca doğru amaç tanımlanır ancak sonraki amaç'ın puanı tanımlanan tutarlı bir şekilde konuşma için önemli ölçüde daha düşük olduğunu doğrulamak için iyi bir yoludur. 

LUIS, birden çok ıntents Kapat tahmin puanları, bir utterance içeriğine göre varsa amaçları arasında geçiş yapabilirsiniz. Bu sorunu gidermek için her amaca daha geniş kitlelere bağlamsal farklar çeşitli konuşma eklemeye devam edin.   

## <a name="e-exponent-notation"></a>E (üstel) gösterimde

Tahmin puanları, üstel gösterim kullanabilir *görünen* 0-1 yukarıda gibi aralığı `9.910309E-07`. Bu puanı göstergesidir bir çok **küçük** sayı.

|E gösterimi puanı |Gerçek puanı|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>Öngörüler farklılıklar
Eğitim doğrulukla öğesi olduğundan farklı bir uygulamada aynı modeli eğitmek ve puanlar bu aynı değildir, bu andır. İkincisi, birden fazla hedefi için bir utterance herhangi bir çakışma üst hedefi aynı utterance için eğitim göre değiştirebilirsiniz anlamına gelir.

Bir güven göstermek için belirli bir LUIS puanı, sohbet botu gerektiriyorsa, bunun yerine üst iki amacı puanı birbirinden kullanmanız gerekir. Bu eğitim çeşitleri için esneklik sağlar. 

## <a name="punctuation"></a>Noktalama işaretleri
Noktalama, LUIS, ayrı bir belirteçtir. Daha önceden bir utterance ve sonunda nokta içeren bir utterance iki ayrı konuşma olan ve iki farklı Öngörüler elde edebilirsiniz. Model emin olun ya da noktalama işleme içinde [örnek konuşma](luis-concept-utterance.md) (sahip ve noktalama işaretleri olmaması) veya [patterns}(luis-concept-patterns.md) noktalama özel söz dizimi ile yok saymak daha kolay olduğu: `I am applying for the {Job} position[.]`

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.