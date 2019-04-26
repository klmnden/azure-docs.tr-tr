---
title: LUIS Portalı'nda test uygulama
titleSuffix: Language Understanding - Azure Cognitive Services
description: Language Understanding (LUIS), uygulamanızı geliştirebilirsiniz ve kendi dil anlama geliştirmek için sürekli olarak çalışmak için kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 51c6a58567b35c9b8486d8634b0bed1af7218994
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60199168"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>LUIS uygulamanızı LUIS portalında test etme
<a name="train-your-app"></a>
[Test](luis-concept-test.md) uygulama yinelemeli bir işlemdir. LUIS uygulamanızı eğitim sonra bu amaç ve varlıkları doğru tanınır olmadığını görmek için örnek Konuşma ile test edin. Değilseniz, güncelleştirmeleri LUIS uygulaması, eğitin ve test için yeniden yapın. 

<!-- anchors for H2 name changes -->
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="test-an-utterance"></a>Bir utterance test

1. Adını seçerek uygulamanıza erişmek **uygulamalarım** sayfası. 

2. Erişim için **Test** slayt genişletme paneli, select **Test** uygulamanızın üst panelinde.

    ![Sayfa eğitme ve Test uygulaması](./media/luis-how-to-interactive-test/test.png)

3. Bir utterance metin kutusuna girin ve Enter tuşuna basın. İstediğiniz gibi konuşma kadar test yazabilirsiniz **Test**, ancak aynı anda yalnızca bir utterance.

4. Utterance, üst amacını ve score konuşma metin kutusunun altında listesine eklenir.

    ![Etkileşimli sınama yanlış amacını tanımlar](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-score"></a>Puan inceleyin

Bir test sonucunun ayrıntılarını incelemek **inceleyin** paneli. 
 
1. İle **Test** slayt genişletme paneli açık, select **inceleyin** karşılaştırmak istediğiniz bir utterance için. 

    ![Test sonuçlarıyla ilgili daha fazla ayrıntı için İncele düğmeyi seçin](./media/luis-how-to-interactive-test/inspect.png)

2. **İnceleme** paneli görüntülenir. Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.

    ![Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Doğru üst hedefi Puanlama

1. Hedefi Puanlama üst yanlışsa, seçin **Düzenle** düğmesi.

2.  Aşağı açılan listesinde, doğru utterance hedefini seçin.

    ![Doğru hedefini seçin](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Yaklaşım sonuçlarını görüntüle

Varsa **yaklaşım analizi** yapılandırılan **[Yayımla](luis-how-to-publish-app.md#enable-sentiment-analysis)** sayfasında test sonuçlarını utterance içinde bulunan yaklaşım içerir. 

![Test Bölmesi ile ilgili yaklaşım analizi görüntüsü](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Eşleşen desenin amacı düzeltin

Kullanıyorsanız [desenleri](luis-concept-patterns.md) ve utterance bir desenle eşleşen, ancak yanlış amacını tahmin, seçin **Düzenle** deseni tarafından bağlamak ve ardından doğru hedefini seçin.

## <a name="compare-with-published-version"></a>Yayımlanan sürümle karşılaştır

Yayımlanan uygulamanızı etkin sürümünü test edebilirsiniz [uç nokta](luis-glossary.md#endpoint) sürümü. İçinde **inceleyin** paneli, select **Karşılaştır yayımlanan**. Yayımlanan modele yönelik test, Azure abonelik kotası bakiyeden çıkarılır. 

![Yayımlanan Karşılaştır](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Uç nokta JSON test panelinde görüntüleyin
JSON seçerek karşılaştırma için döndürülen uç nokta görüntüleyebileceğiniz **Göster JSON görünümü**.

![Yayımlanan bir JSON yanıtı](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Test panelinde ek ayarlar

### <a name="luis-endpoint"></a>LUIS uç noktası

Birkaç LUIS uç noktaları varsa, **ek ayarlar** Test bağlantısını test etmek için kullanılan uç nokta değiştirmek için bölmesinde yayımlanan. Kullanmak için hangi uç noktaya emin değilseniz varsayılan seçin **Starter_Key**. 

![Ek ayarlar bağlantısının vurgulandığı test paneli](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)


### <a name="view-bing-spell-check-corrections-in-test-panel"></a>Bing yazım denetimi düzeltmeleri test panelinde görüntüleyin

Yazım düzeltmeleri görmek için gereksinimler: 

* Yayımlanan uygulama
* Bing yazım denetimi [hizmet anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api). Hizmet anahtarı depolanmayacak ve her bir tarayıcı oturumu için sıfırlanması gerekir. 

Dahil etmek için aşağıdaki yordamı kullanın. [Bing yazım denetimi v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) Test sonuçları bölmesinde hizmet. 

1. İçinde **Test** bölmesinde bir utterance girin. Utterance tahmin bittiğinde **[inceleyin](#inspect-score)** girdiğiniz utterance altında. 

2. Zaman **inceleyin** panelini açar, seçim  **[ile yayımlanan karşılaştırma](#compare-with-published-version)**. 

3. Zaman **yayımlanan** panelini açar, seçim  **[ek ayarlar](#additional-settings-in-test-panel)**.

4. Açılan iletişim kutusuna girin, **Bing yazım denetimi** hizmet anahtarı. 
    ![Bing yazım denetimi hizmet anahtarı girin](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)

5. Bir hatalı gibi yazım denetimi ile bir sorgu girin `book flite to seattle` seçin girin. Sözcük yanlış yazımını `flite` LUIS için gönderilen sorgu değiştirilir ve sonuçta elde edilen JSON olarak hem özgün sorguyu gösterir. `query`ve düzeltilmiş sorgu yazımı olarak `alteredQuery`.

    ![JSON yazım denetimi düzeltildi](./media/luis-how-to-interactive-test/interactive-with-spell-check-results.png)

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="batch-testing"></a>Toplu işe testi
Toplu test bkz [kavramları](luis-concept-batch-test.md) ve öğrenin [nasıl](luis-how-to-batch-test.md) konuşma toplu test edin.

## <a name="next-steps"></a>Sonraki adımlar

Test LUIS uygulamanızı doğru amaç ve varlıkları tanımıyor gösteriyorsa, daha fazla konuşma etiketleme ya da özellikler ekleyerek LUIS uygulamanızın doğruluğunu artırmak için çalışabilir. 

* [LUIS ile önerilen konuşma etiket](luis-how-to-review-endpoint-utterances.md) 
* [LUIS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md) 
