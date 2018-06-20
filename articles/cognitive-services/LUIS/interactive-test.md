---
title: Eğitmek ve HALUK uygulamanızı - Azure test | Microsoft Docs
description: Sürekli olarak geliştirebilirsiniz ve kendi dil anlama geliştirmek için uygulamanızın üzerinde çalışmak için dil anlama (HALUK) kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: fb4c3bb117d1ea60c9cc28d2b193ee3c01f6c945
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221640"
---
# <a name="test-your-luis-app"></a>HALUK uygulamanızı test etme
<a name="train-your-app"></a>
[Eğitim](luis-how-to-train.md) ve [sınama](luis-concept-test.md) bir uygulama yinelemeli bir işlemdir. HALUK uygulamanızı eğitim sonra hedefleri ve varlıkları doğru tanınan olmadığını görmek için örnek utterances ile sınayın. Değilseniz, güncelleştirmelerinin HALUK uygulaması, eğitme ve test için yeniden olması. 

<!-- anchors for H2 name changes -->
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="interactive-testing"></a>
## <a name="test-an-utterance"></a>Bir utterance test

1. Şirket adını seçerek uygulamanızı erişim **My uygulamaları** sayfası. 

2. Erişim için **Test** slayt çıkış paneli, select **Test** uygulamanızın üst panelinde.

    ![Tren & Test uygulama sayfası](./media/luis-how-to-interactive-test/test.png)

3. Bir utterance metin kutusuna girin ve Enter seçin. İstediğiniz kadar test utterances yazabilirsiniz **Test**, ancak aynı anda yalnızca bir utterance.

4. Utterance, üst amacını ve puanı utterances metin kutusu altında listesine eklenir.

    ![Etkileşimli sınama yanlış amacını tanımlar](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="clear-test-panel"></a>Clear test paneli
Tüm girilen test utterances ve sonuçları test konsolundan temizlemek için seçin **baştan başlamak** sol üst köşesindeki **Test paneli**. 

## <a name="close-test-panel"></a>Kapat test paneli
Kapatmak için **Test** paneli, select **Test** yeniden düğmesine tıklayın.

## <a name="inspect-score"></a>Puan inceleyin.
Test sonucu ayrıntılarını incelemek **incele** paneli. 
 
1. İle **Test** slayt çıkış paneli açın, select **incele** karşılaştırmak istediğiniz utterance için. 

    ![Düğme inceleyin.](./media/luis-how-to-interactive-test/inspect.png)

2. **Denetleme** panelinde görüntülenir. Bölmenin hedefi olarak tanımlanan herhangi bir varlık Puanlama üst içerir. Bölmenin seçili utterance sonucunu gösterir.

    ![Düğme inceleyin.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Hedefi Puanlama doğru üst

1. Hedefi Puanlama üst yanlış ise, seçin **Düzenle** düğmesi.

2.  Aşağı açılan listeden utterance doğru hedefini seçin.

    ![Doğru hedefini seçin](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Düşünceleri sonuçları görüntüleme

Varsa **düşünceleri analiz** yapılandırılan **[Yayımla](publishapp.md#enable-sentiment-analysis)** sayfasında test sonuçlarını utterance bulunan düşünceleri içerir. 

![Test bölmesi düşünceleri analiz görüntüsü](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Eşleşen deseninin hedefi düzeltin
Kullanıyorsanız [desenleri](luis-concept-patterns.md) ve utterance bir desenle eşleşen ancak yanlış hedefi tahmin, seçin **Düzenle** bağlantı desen sonra doğru hedefini seçin.

## <a name="compare-with-published-version"></a>Yayımlanmış bir sürüm ile karşılaştırın
Yayımlanan ile uygulamanızı etkin sürümü test edebilir [endpoint](luis-glossary.md#endpoint) sürümü. İçinde **incele** paneli, select **ile karşılaştırın yayımlanan**. Yayımlanan modeline göre sınama Azure aboneliği kota bakiyeniz çıkarılır. 

![Yayımlanan ile karşılaştırın](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Test panelinde görünüm endpoint JSON
JSON seçerek karşılaştırma için döndürülen uç nokta görüntüleyebilirsiniz **Göster JSON Görünüm**.

![Yayımlanan JSON yanıtı](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Test paneli ek ayarlar

### <a name="luis-endpoint"></a>HALUK uç noktası
Birkaç HALUK uç noktaları varsa, **ek ayarlar** bağlantıyı test bölmesi test etmek için kullanılan uç noktayla değiştirmek için yayımlanan. Kullanmak için hangi uç noktaya emin değilseniz varsayılan seçmek **Starter_Key**. 

![Vurgulanan ek ayarlar bağlantıyla test paneli](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)


### <a name="view-bing-spell-check-corrections-in-test-panel"></a>Bing yazım denetimi düzeltmelerini test panelinde görüntüleyin
Yazım denetimi düzeltmeleri görüntülemek için gereksinimler: 

* Yayımlanan uygulama
* Bing yazım denetimi [hizmet anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api). Hizmet anahtarı olmayan depolanır ve her bir tarayıcı oturumu için sıfırlanması gerekir. 

Dahil etmek için aşağıdaki yordamı kullanın [Bing yazım denetimi v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) Test bölmesi sonuçları hizmet. 

1. İçinde **Test** bölmesinde, bir utterance girin. Utterance tahmin, seçin **[incele](#inspect-score)** girdiğiniz utterance altında. 

2. Zaman **incele** paneli açılır, select  **[yayımlanan ile karşılaştırmak](#compare-with-published-version)**. 

3. Zaman **yayımlanan** paneli açılır, select  **[ek ayarlar](#additional-settings-in-test-panel)**.

4. Açılan iletişim kutusunda girin, **Bing yazım denetimi** hizmet anahtarı. 
    ![Bing yazım denetimi hizmet anahtarı girin](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)

5. Bir hatalı gibi yazım denetimi ile bir sorgu girin `book flite to seattle` ve select girin. Word yanlış yazım `flite` HALUK için gönderilen sorgusunda değiştirilir ve sonuçta elde edilen JSON olarak hem orijinal sorguyu gösterir `query`ve sorgu düzeltilmiş yazım olarak `alteredQuery`.

    ![JSON yazım düzeltildi](./media/luis-how-to-interactive-test/interactive-with-spell-check-results.png)

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
## <a name="batch-testing"></a>Toplu test etme
Toplu test bkz [kavramları](luis-concept-batch-test.md) ve öğrenin [nasıl](luis-how-to-batch-test.md) utterances toplu sınayın.

## <a name="next-steps"></a>Sonraki adımlar

Sınama HALUK uygulamanızın doğru hedefleri ve varlıkları tanımıyor gösteriyorsa, daha fazla utterances etiketleme ya da özellikler ekleyerek HALUK uygulamanızın doğruluğunu artırmak için çalışabilir. 

* [Önerilen utterances HALUK ile etiket](Label-Suggested-Utterances.md) 
* [HALUK uygulamanızın performansını artırmak için özelliklerini kullanma](luis-how-to-add-features.md) 
