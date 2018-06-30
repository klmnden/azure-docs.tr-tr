---
title: Bing yazım denetleme API v7 HALUK sorguları için ekleme | Microsoft Docs
titleSuffix: Azure
description: Bing yazım denetleme API V7 HALUK uç nokta sorguları ekleyerek utterances doğru yanlış yazılmış sözcüklerin.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/27/2018
ms.author: v-geberr
ms.openlocfilehash: 96b23146e726b7fee86b7e449c81d7efc0073e8d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127678"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Bing yazım denetimi ile doğru sözcüklerin

HALUK uygulamanızla tümleştirin [Bing yazım denetleme API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) HALUK puan ve utterance varlıklarının tahmin önce utterances yanlış yazılmış sözcüklerin düzeltmek için. 

## <a name="create-first-key-for-bing-spell-check-v7"></a>Bing yazım denetleme V7 için ilk anahtarı oluşturma
[İlk Bing yazım denetleme API v7 anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) ücretsizdir. 

![Ücretsiz anahtarı oluşturma](./media/luis-tutorial-bing-spellcheck/free-key.png)

< bir adı "-abonelik-anahtarı oluştur" ></a>
## <a name="create-endpoint-key"></a>Uç noktası anahtarı oluşturma
Ücretsiz anahtarınızı süresi varsa, bir uç noktası anahtarı oluşturun.

1. [Azure Portal](https://portal.azure.com)’da oturum açın. 

2. Seçin **kaynak oluşturma** sol üst köşesindeki.

3. Arama kutusuna `Bing Spell Check API V7` yazın.

    ![Bing yazım ara API V7 denetleyin](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Hizmeti seçin. 

5. Bir bilgi panelini yasal bildirim gibi bilgileri içeren sağ görünür. Seçin **oluşturma** abonelik oluşturma işlemine başlamak için. 

6. Sonraki panelinde, hizmet ayarlarını girin. Hizmet oluşturma işleminin tamamlanması için bekleyin.

    ![Hizmet ayarlarını girin](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Seçin **tüm kaynakları** altında **Sık Kullanılanlar** sol taraftaki gezinti başlığı.

8. Yeni hizmeti seçin. Kendi türü **Bilişsel Hizmetler** ve konum **genel**. 

9. Ana panelinde seçin **anahtarları** yeni anahtarlarınızı görmek için.

    ![Anahtarları alın](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. İlk anahtarı kopyalayın. Yalnızca iki anahtarlarından birini gerekir. 

## <a name="using-the-key-in-luis-test-panel"></a>HALUK test panelinde anahtarı kullanma
HALUK anahtarı kullanmak üzere iki yerde vardır. İlk olarak [test paneli](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). Anahtar HALUK kaydedilmez, ancak bunun yerine oturum bir değişkendir. Bing yazım denetleme API v7 hizmeti utterance uygulamak için test paneli istediğiniz her zaman anahtar ayarlamanız gerekir. Bkz: [yönergeleri](interactive-test.md#view-bing-spell-check-corrections-in-test-panel) anahtarını ayarlamak için test panelinde.

## <a name="adding-the-key-to-the-endpoint-url"></a>Anahtar uç noktası URL'ye ekleniyor
Uç nokta sorgu yazım düzeltme uygulamak istediğiniz her sorgu için sorgu dizesi parametrelerinde geçirilen anahtar gerekir. HALUK çağıran bir chatbot sahip olabilir veya API HALUK uç noktası doğrudan çağırabilir. Uç nokta nasıl adlandırılır bağımsız olarak her çağrı yazım düzeltmeleri düzgün çalışması için gerekli bilgiler içermelidir.

Uç nokta URL'si doğru geçirilmesi gereken birkaç değerlere sahip. Bing yazım denetleme API v7 bunlardan yalnızca başka bir anahtardır. Ayarlamanız gerekir **yazım denetimi** parametresi true ve değerini ayarlamalısınız **bing yazım-onay-abonelik-anahtar** anahtar değeri için:

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appID}?Subscription-Key={luisKey}&spellCheck=**true**& bing-yazım-onay-subscription-key =**{bingKey}**& ayrıntılı true & timezoneOffset = 0 & q = {utterance} =

## <a name="send-misspelled-utterance-to-luis"></a>Yanlış yazılmış utterance HALUK için Gönder
1. Bir web tarayıcısında önceki dizesini kopyalayın ve değiştirme `region`, `appId`, `luisKey`, ve `bingKey` kendi değerlere sahip. Yayımlama farklıysa uç nokta bölgesini kullandığınızdan emin olun [bölge](luis-reference-regions.md).

2. Yanlış yazılmış utterance gibi "ne kadar mountainn?" ekleyin. İngilizce, `mountain`, biriyle `n`, doğru yazım değil. 

3. HALUK için sorgu göndermeyi seçin girin.

4. HALUK yanıt için bir JSON sonuçla `How far is the mountain?`. Bing yazım denetleme API v7 bir yazım hatası algılarsa `query` alan HALUK uygulamanın JSON yanıt işleminin orijinal sorguyu içerir ve `alteredQuery` alan HALUK için gönderilen düzeltilmiş sorgu içerir.

```
{
  "query": "How far is the mountainn?",
  "alteredQuery": "How far is the mountain?",
  "topScoringIntent": {
    "intent": "Concierge",
    "score": 0.183866
  },
  "entities": []
}
```

## <a name="ignore-spelling-mistakes"></a>Yazım yoksay
Bing yazım denetleme API v7 hizmetini kullanmak istemiyorsanız, yazım hatalarını sahip ve böylece HALUK uygun yazım ve bunun yanı sıra yazım hatalarını öğrenebilirsiniz utterances etiketleyebilirsiniz. Bu seçenek, bir yazım denetleyicisi kullanmaktan daha fazla etiketleme çaba gerektirir.

## <a name="publishing-page"></a>Yayımlama Sayfası
[Yayımlama](publishapp.md) sayfasına sahip bir **etkinleştirmek Bing yazım denetleyicisi** onay kutusu. Bu anahtarı oluşturun ve uç nokta URL'sini nasıl değiştiğini anlamak için kullanışlı olur. Her utterance için düzeltildi yazım olması için doğru uç nokta parametrelerini kullanmak çözümlenmedi. 

> [!div class="nextstepaction"]
> [Örnek utterances hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
