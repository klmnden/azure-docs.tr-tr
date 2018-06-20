---
title: HALUK - Azure tarafından döndürülen tahmin puan anlama | Microsoft Docs
description: İçinde HALUK tahmin puan anlamı öğrenin
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 31c101a23892df8599b8cdc0f67647fefb969490
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265997"
---
# <a name="prediction-score"></a>Tahmin puanı
Tahmin puanı HALUK tahmin sonuçlarını sahip güvenirlik düzeyini gösterir. 

Tahmin puan genellikle sıfır (0) ve bir (1) olur. Bir yüksek oranda emin HALUK puan 0.99 örnektir. 0,01 puanı düşük güvenilirlik, örnektir. 

|Puan değeri|Güven|
|--|--|
|1|kesin eşleşmiyor|
|0.99|yüksek güvenilirlik|
|0.01|Düşük güvenilirlik|
|0|eşleştirilecek kesin hatası|

Bir utterance bir düşük güvenilirlik puan sonuçlandığında HALUK uygulamasında vurgular [HALUK] [ LUIS] Web sitesi **hedefi** ile tanımlanan, sayfa **etiketli hedefi**  kırmızı ile ana hatlarıyla. 

![Puan tutarsızlık](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Üst Puanlama hedefi
Her utterance tahmin üst Puanlama amacıyla döndürür. Tahmin puanları bir sayısal karşılaştırması budur. İlk iki puanları aralarında çok küçük bir fark olabilir. HALUK puanları döndürme dışında bu yakınlık göstermez.  

Üst puanları yakınlık hakkında endişeleriniz varsa, tüm hedefleri puanını döndürmelidir. Word seçiminde farklılıkları gösteren iki amaçlar için utterances ekleyebilirsiniz ve düzenleme veya bir chatbot gibi HALUK arama uygulaması varsa, iki üst hedefleri nasıl ele alınacağını hakkında programlı seçimler. 

## <a name="return-prediction-score-for-all-intents"></a>Tüm amaçlar için tahmini puan döndürür
Bir test veya uç nokta sonuç tüm hedefleri içerebilir. Bu yapılandırma üzerinde ayarlanmış [endpoint](https://aka.ms/v1-endpoint-api-docs) ile `verbose=true` sorgu dizesi ad/değer çifti. 

## <a name="review-intents-with-similar-scores"></a>Benzer puanları ile hedefleri gözden geçirin
Tüm amaçlar için puan gözden geçirme, yalnızca doğru hedefi tanımlanır ancak sonraki hedefi'nın puan tanımlanan utterances için tutarlı bir şekilde önemli ölçüde daha düşük olduğunu doğrulamak için iyi bir yöntemidir. 

Birden çok hedefleri bir utterance bağlamında Kapat tahmin puanları varsa HALUK hedefleri arasında geçiş yapabilirsiniz. Bu sorunu gidermek için her amacıyla çok çeşitli bağlamsal farklar için utterances eklemeye devam edin.   

## <a name="e-exponent-notation"></a>E (üs) gösterimi

Tahmin puanları üstel gösterimde, kullanabileceğiniz *görünen* 0-1 yukarıda aralığı, gibi `9.910309E-07`. Bu puan bir göstergesidir bir çok **küçük** numarası.

|E gösterimi puanı |Gerçek puanı|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>Tahminleri farklılıklar
Eğitim rastgele bir öğe olduğundan farklı bir uygulamayı aynı modelde eğitmek ve notların bu aynı değildir, bu değildir. İkincisi, herhangi bir utterance birden çok amaç için çakışmasını aynı utterance üst hedefini eğitim göre değiştirebilirsiniz anlamına gelir.

Chatbot amacına güveni göstermek için belirli bir HALUK puan gerektiriyorsa, bunun yerine üst iki hedefleri puan birbirinden kullanmanız gerekir. Bu eğitim Çeşitlemeler için esneklik sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [varlıkları ekleyin](luis-how-to-add-entities.md) HALUK uygulamanıza varlıklar ekleme hakkında daha fazla bilgi için.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions