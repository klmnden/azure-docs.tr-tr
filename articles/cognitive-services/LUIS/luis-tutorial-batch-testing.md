---
title: LUIS tahminleri artırmak için toplu test kullanma | Microsoft Docs
titleSuffix: Azure
description: Yük toplu test sonuçlarını gözden geçirin ve değişikliklerle LUIS tahminleri geliştirmek.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 27d6bbc628ac3183032a90d8f3ad98998c76a957
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888839"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Toplu test tahmin doğruluğunu sorunları bulmak için kullanın

Bu öğreticide, batch test utterance tahmin sorunları bulmak için nasıl kullanılacağı gösterilmektedir.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Toplu test dosyası oluşturma 
* Toplu test çalıştırma
* Test sonuçlarını gözden geçirme
* Hedefleri için hataları düzeltin
* Batch yeniden test et

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Bu makalede ayrıca gerekir bir [LUIS](luis-reference-regions.md) LUIS uygulamanızı yazmak için kullanılan hesap.

> [!Tip]
> Zaten bir aboneliğiniz yoksa, kaydolabilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

## <a name="create-new-app"></a>Yeni uygulama oluşturma
Bu makalede önceden oluşturulmuş etki alanı HomeAutomation kullanır. Amacı, varlıkları ve konuşma gibi ışıklar HomeAutomation cihazları denetlemek için önceden oluşturulmuş etki alanı vardır. Uygulamayı oluşturur, etki alanı ekleme eğitin ve yayımlayın.

1. İçinde [LUIS](luis-reference-regions.md) Web sitesi, seçerek yeni bir uygulama oluşturun **yeni uygulama oluştur** üzerinde **MyApps** sayfası. 

    ![Yeni uygulama oluşturma](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Bir ad girin `Batchtest-HomeAutomation` iletişim.

    ![Uygulama adı girin](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Seçin **önceden oluşturulmuş etki alanları** sol alt köşedeki. 

    ![Önceden oluşturulmuş etki alanını seçin](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Seçin **etki alanı ekleme** HomeAutomation için.

    ![HomeAutomation etki alanı ekleme](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Seçin **eğitme** üst sağ gezinti çubuğunda.

    ![Train düğmeyi seçin](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Batch sınama ölçütünde
Batch sınama aynı anda en fazla 1000 konuşma test edebilirsiniz. Toplu işlem yinelenen sahip olmamalıdır. [Dışarı aktarma](create-new-app.md#export-app) geçerli konuşma listesini görmek için uygulama.  

LUIS test stratejisi, verilerin üç ayrı kümelerini kullanır: model utterances, toplu test utterances ve uç nokta utterances. Bu öğreticide, konuşma (bir amaç için eklenir) ya da model konuşma gelen veya konuşma uç noktası kullanmadığınızdan emin olun. 

Herhangi bir konuşma zaten uygulamada toplu test için kullanmayın:

```
'breezeway on please',
'change temperature to seventy two degrees',
'coffee bar on please',
'decrease temperature for me please',
'dim kitchen lights to 25 .',
'fish pond off please',
'fish pond on please',
'illuminate please',
'living room lamp on please',
'living room lamps off please',
'lock the doors for me please',
'lower your volume',
'make camera 1 off please',
'make some coffee',
'play dvd',
'set lights bright',
'set lights concentrate',
'set lights out bedroom',
'shut down my work computer',
'silence the phone',
'snap switch fan fifty percent',
'start master bedroom light .',
'theater on please',
'turn dimmer off',
'turn off ac please',
'turn off foyer lights',
'turn off living room light',
'turn off staircase',
'turn off venice lamp',
'turn on bathroom heater',
'turn on external speaker',
'turn on my bedroom lights .',
'turn on the furnace room lights',
'turn on the internet in my bedroom please',
'turn on thermostat please',
'turn the fan to high',
'turn thermostat on 70 .' 
```

## <a name="create-a-batch-to-test-intent-prediction-accuracy"></a>Hedefi tahmin doğruluğunu test etmek için bir toplu iş oluşturma
1. Oluşturma `homeauto-batch-1.json` gibi bir metin düzenleyicisinde [VSCode](https://code.visualstudio.com/). 

2. Konuşma ile ekleme **hedefi** testinde tahmin edilen istiyor. Bu öğreticide, basit hale getirmek üzere konuşma olur `HomeAutomation.TurnOn` ve `HomeAutomation.TurnOff` ve geçiş `on` ve `off` konuşma metin. İçin `None` amacı, birkaç olmayan Konuşma ekleme parçası [etki alanı](luis-glossary.md#domain) (konu) alanı. 

    Toplu test sonuçlarını toplu JSON nasıl ilişkilendirmek anlamak için yalnızca altı ıntents ekleyin.

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>Toplu işlem çalıştırın
1. Seçin **Test** üst gezinti çubuğunda. 

    ![Gezinti bölmesinde testi seçin](./media/luis-tutorial-batch-testing/test-1.png)

2. Seçin **test paneli toplu** Sağdaki panelde. 

    ![Toplu test panosunu seçin](./media/luis-tutorial-batch-testing/test-2.png)

3. Seçin **alma dataset**.

    ![İçeri aktarma veri kümesi seçin](./media/luis-tutorial-batch-testing/test-3.png)

4. Dosya sistemi konumunu seçin `homeauto-batch-1.json` dosya.

5. Veri kümesi adı `set 1`.

    ![Dosya seç](./media/luis-tutorial-batch-testing/test-4.png)

6. **Çalıştır** düğmesini seçin. Test işlemi tamamlanana kadar bekleyin.

    ![Çalıştır'ı seçin](./media/luis-tutorial-batch-testing/test-5.png)

7. Seçin **bkz sonuçları**.

    ![Sonuçları göster](./media/luis-tutorial-batch-testing/test-6.png)

8. Gösterge ve grafik sonuçlarını gözden geçirin.

    ![Toplu işlem sonuçları](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Toplu iş sonuçlarını gözden geçirin
Toplu sonuçları iki bölümü var. Üst kısmında, graf ve gösterge içerir. Bir grafik alanı adını seçtiğinizde alt kısmında konuşma görüntüler.

Hatalar kırmızı renk tarafından belirtilir. İki kırmızı renkte gösterilir bölümlerin dört bölümde grafiğidir. **Temel odak noktası bölümlere bunlar**. 

Bölüm yanlış test gösterir sağ üst bir hedefi veya varlık varlığını tahmin. Sol alt kısmında, yanlış tahmin edilen bir hedefi veya varlık test gösterir.

### <a name="homeautomationturnoff-test-results"></a>HomeAutomation.TurnOff test sonuçları
Göstergede seçin `HomeAutomation.TurnOff` hedefi. Bu açıklama bölümünde adının solunda bir yeşil bir başarı simgesi vardır. Bu amaç için hata yok. 

![Toplu işlem sonuçları](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>HomeAutomation.TurnOn ve hiçbiri hedefleri olan hataları
Diğer iki amacı hatalar, test Öngörüler toplu dosya beklentileri eşleşmedi anlamı vardır. Seçin `None` ilk hata gözden geçirmek için göstergede hedefi. 

![Hedefi yok](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Hatalar görünür kırmızı bölümlerde grafikteki: **yanlış pozitif** ve **False negatif**. Seçin **False negatif** grafiğin altındaki başarısız konuşma görmek için grafikteki bölüm adı. 

![False negatif hataları](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

Başarısız olan utterance `help` olarak beklenen bir `None` hedefi ancak tahmin edilen test `HomeAutomation.TurnOn` hedefi.  

İki hataları HomeAutomation.TurnOn birinde ve hiçbiri birinde vardır. Her ikisi tarafından utterance kaynaklanan `help` çünkü hiçbiri beklentisi karşılamak başarısız oldu ve HomeAutomation.TurnOn amaç için beklenmeyen bir eşleşme oluştu. 

Nedenini belirlemek için `None` konuşma testlerden, şu anda konuşma gözden `None`. 

## <a name="review-none-intents-utterances"></a>Gözden geçirme konuşma kullanıcının hedefi yok

1. Kapat **Test** seçerek paneli **Test** üst gezinti çubuğunda düğme. 

2. Seçin **derleme** üst gezinti panelinde. 

3. Seçin **hiçbiri** hedefi amaçlarını listesinden.

4. Konuşma belirteci görünümünü görmek için kontrol + E seçin 
    
    |Konuşma kullanıcının hedefi yok|Tahmin puanı|
    |--|--|
    |"Sıcaklık benim için lütfen azaltma"|0.44|
    |"25 mutfak ışıkları dim."|0.43|
    |"toplu alt"|0.46|
    |"Benim Yatak odası Lütfen internet'teki kapatma"|0.28|

## <a name="fix-none-intents-utterances"></a>Düzeltme hedefi yok konuşma kullanıcının
    
İçinde herhangi bir konuşma `None` uygulama etki alanı dışındaki olması gerekiyor. Bu konuşma HomeAutomation göreli olduğundan yanlış amaca oldukları. 

LUIS ayrıca verir % 50'değerinden küçük utterances (<.50) tahmin puanı. Diğer iki amacı, konuşma bakarsanız, çok daha yüksek tahmin puanları bakın. LUIS anlarsınız örnek utterances, düşük puanlarını olduğunda utterances geçerli amacını ve diğer amaçlar arasında için LUIS karmaşıktır. 

Uygulama şu anda konuşma düzeltmek için `None` hedefi ihtiyaç doğru amaç taşınacak ve `None` hedefi yeni, uygun bir ıntents gerekiyor. 

Üçü konuşma `None` hedefi Otomasyon cihaz ayarları düşürmek için geliyordu. Sözcükler gibi kullandıkları `dim`, `lower`, veya `decrease`. Dördüncü utterance Internet üzerinde etkinleştirmek ister. Tüm dört konuşma açma veya güç derecesini bir cihaza değiştirme hakkında olduğundan, bunlar için kaydırılacağı `HomeAutomation.TurnOn` hedefi. 

Bu tek bir çözümdür. Yeni bir amacı da oluşturabilirsiniz `ChangeSetting` dim, kullanarak konuşma taşıma alt ve bu yeni hedefi azaltın. 

## <a name="fix-the-app-based-on-batch-results"></a>Batch sonuçlarına göre uygulama düzeltme
Dört konuşma taşıma `HomeAutomation.TurnOn` hedefi. 

1. Tüm konuşma seçili şekilde utterance listenin üstündeki onay kutusu seçin. 

2. İçinde **yeniden atama hedefi** açılan listesinde, select `HomeAutomation.TurnOn`. 

    ![Konuşma Taşı](./media/luis-tutorial-batch-testing/move-utterances.png)

    Dört konuşma atanır sonra utterance listelemek için `None` hedefi boştur.

3. Dört yeni hedef hiçbiri için hedefi ekleme:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Bu konuşma kesinlikle HomeAutomation etki alanı dışında olan. Her utterance girerken, puan için izleyin. Puan, düşük veya hatta çok düşük (kırmızı kutu çevresinde) sahip olabilir. Adım 8'de uygulamayı eğitme sonra puan çok daha yüksek olur. 

7. Mavi etiket utterance ve select seçerek tüm etiketleri kaldırmak **Etiketi Kaldır**.

8. Seçin **eğitme** üst sağ gezinti çubuğunda. Her utterance puanı çok daha yüksektir. Tüm puanları `None` hedefi olmalıdır.80 şimdi. 

## <a name="verify-the-fix-worked"></a>Düzeltme çalışılan doğrulayın
İçin toplu test konuşma doğru şekilde tahmin doğrulamak için **hiçbiri** amacı, batch testi yeniden çalıştırın.

1. Seçin **Test** üst gezinti çubuğunda. 

2. Seçin **test paneli toplu** Sağdaki panelde. 

3. Öğesinin üç noktasını (***...*** ) sağında toplu işlem adı'düğmesine tıklayın ve belirleyin **çalıştırma Dataset**. Toplu test işlemi tamamlanana kadar bekleyin.

    ![Veri kümesi çalıştırın](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Seçin **bkz sonuçları**. Intents tüm hedefi adları solundaki yeşil simgeler olması gerekir. Ayarlamak doğru filtreyle `HomeAutomation.Turnoff` yeşil nokta grafiğinin ortasını için en yakın üst sağ panelde hedefini seçin. Grafiğin altındaki tabloda utterance adı görünür. Puanı `breezeway off please` çok düşüktür. İsteğe bağlı bir etkinliği, bu puanı artırmak için hedefi için daha fazla konuşma eklemektir. 

    ![Veri kümesi çalıştırın](./media/luis-tutorial-batch-testing/turnoff-low-score.png)

<!--
    The Entities section of the legend may have errors. That is the next thing to fix.

## Create a batch to test entity detection
1. Create `homeauto-batch-2.json` in a text editor such as [VSCode](https://code.visualstudio.com/). 

2. Utterances have entities identified with `startPos` and `endPost`. These two elements identify the entity before [tokenization](luis-glossary.md#token), which happens in some [cultures](luis-supported-languages.md#tokenization) in LUIS. If you plan to batch test in a tokenized culture, learn how to [extract](luis-concept-data-extraction.md#tokenized-entity-returned) the non-tokenized entities.

    Copy the following JSON into the file:

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 4
            }
          ]
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Operation",
              "startPos": 7,
              "endPos": 17
            }
          ]
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 9
            }
          ]
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Device",
              "startPos": 0,
              "endPos": 10
            }
          ]
        }
      ]
    ```

3. Import the batch file, following the [same instructions](#run-the-batch) as the first import, and name the dataset `set 2`. Run the test.

## Possible entity errors
Since the intents in the right-side filter of the test panel still pass the test, this section focuses on correct entity identification. 

Entity testing is diferrent than intents. An utterance will have only one top scoring intent, but it may have several entities. An utterance's entity may be correctly identified, may be incorrectly identified as an entity other than the one in the batch test, may overlap with other entities, or not identified at all. 

## Review entity errors
1. Select `HomeAutomation.Device` in the filter panel. The chart changes to show a single false positive and several true negatives. 

2. Select the False positive section name. The utterance for this chart point is displayed below the chart. The labeled intent and the predicted intent are the same, which is consistent with the test -- the intent prediction is correct. 

    The issue is that the HomeAutomation.Device was detected but the batch expected HomeAutomation.Room for the utterance "coffee bar off please". `Coffee bar` could be a room or a device, depending on the environment and context. As the model designer, you can either enforce the selection as `HomeAutomation.Room` or change the batch file to use `HomeAutomation.Device`. 

    If you want to reinforce that coffee bar is a room, you nee to add an utterances to LUIS that help LUIS decide a coffee bar is a room. 

    The most direct route is to add the utterance to the intent but that to add the utterance for every entity detection error is not the machine-learned solution. Another fix would be to add an utterance with `coffee bar`.

## Add utterance to help extract entity
1. Select the **Test** button on the top navigation to close the batch test panel.

2. On the `HomeAutomation.TurnOn` intent, add the utterance, `turn coffee bar on please`. The uttterance should have all three entities detected after you select enter. 

3. Select **Train** on the top navigation panel. Wait until training completes successfully.

3. Select **Test** on the top navigation panel to open the Batch testing pane again. 

4. If the list of datasets is not visible, select **Back to list**. Select the ellipsis (***...***) button at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek konuşma hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions