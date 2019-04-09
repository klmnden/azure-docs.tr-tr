---
title: Uygulamayı eğitme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Eğitim, doğal dil anlama geliştirmek için Language Understanding (LUIS) uygulama sürümü eğitiminde işlemidir. Ekleme, düzenleme, etiketleme veya varlıkları, amacı veya konuşma silme gibi bir Modeli'ne güncelleştirmelerinden sonra LUIS uygulamanızı eğitin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/07/2019
ms.author: diberry
ms.openlocfilehash: ba0db22437961a33b0b415ec7cb60ad3df12821c
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267064"
---
# <a name="train-your-active-version-of-the-luis-app"></a>Etkin LUIS uygulaması sürümünüz eğitin 

Eğitim, doğal dil anlama geliştirmek için Language Understanding (LUIS) uygulamanızı eğitiminde işlemidir. Ekleme, düzenleme, etiketleme veya varlıkları, amacı veya konuşma silme gibi bir Modeli'ne güncelleştirmelerinden sonra LUIS uygulamanızı eğitin. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Eğitim ve [test](luis-concept-test.md) uygulama yinelemeli bir işlemdir. LUIS uygulamanızı eğitme sonra varlıkları ve hedefleri doğru olarak tanınır olmadığını görmek için örnek Konuşma ile test edin. Değilseniz, güncelleştirmeleri LUIS uygulaması, eğitin ve test için yeniden yapın. 

Eğitim LUIS Portalı'nda etkin sürüme uygulanır. 

## <a name="how-to-train-interactively"></a>Etkileşimli olarak eğitme

İçinde bir süreçtir başlatmak için [LUIS portalı](https://www.luis.ai), ilk LUIS uygulamanızı en az bir kez eğitmek gerekir. Eğitim önce en az bir utterance her hedefi olduğundan emin olun.

1. Adını seçerek uygulamanıza erişmek **uygulamalarım** sayfası. 

2. Uygulamanızda seçin **eğitme** üst panelinde. 

3. Alıştırma tamamlandıktan sonra yeşil bildirim çubuğu tarayıcı üst kısmında görünür.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Örnek konuşma içermeyen bir veya daha fazla ıntents uygulamanızda varsa, uygulamanızı eğitme olamaz. Tüm amaçlar için konuşma ekleyin. Daha fazla bilgi için [örnek Konuşma ekleme](luis-how-to-add-example-utterances.md).

## <a name="training-date-and-time"></a>Eğitim tarih ve saat

Eğitim tarih ve saat GMT + 2 şeklindedir. 

## <a name="train-with-all-data"></a>Tüm verilerle eğitim

Eğitim negatif örnekleme küçük bir yüzdesine kullanır. Tüm verileri küçük negatif örnekleme yerine kullanmak istediğiniz kullanırsanız [sürüm ayarları API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) ile `UseAllTrainingData` kapat Bu özellik True olarak ayarlayın. 

## <a name="unnecessary-training"></a>Gereksiz eğitim

Tek her değişiklikten sonra eğitme gerekmez. Eğitim, sonra bir grup değişiklikleri modele uygulanır ve test etmek veya yayımlamak için yapmanız gereken sonraki adım olan yapılmalıdır. Eğitim, test veya yayımlamak ihtiyacınız yoksa, gerekli değildir. 

## <a name="training-with-the-rest-apis"></a>REST API'leri ile eğitim

LUIS portalında eğitim tuşlarına basarak, tek bir adım olduğunu **eğitme** düğmesi. REST API'leri ile eğitim iki adımlı bir işlemdir. Birincisi [istek eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) HTTP POST ile. Daha sonra istek [eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) HTTP Get ile. 

Eğitim tamamlandığında öğrenmek için tüm modelleri başarıyla eğitilir kadar durum yoklaması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

* [LUIS ile önerilen konuşma etiket](luis-how-to-review-endpoint-utterances.md) 
* [LUIS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md) 
