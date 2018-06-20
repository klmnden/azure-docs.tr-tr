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
ms.openlocfilehash: 5788f17f2724a0354a1db506971c2343c1800f01
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266405"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Tahmin doğruluğunu sorunları bulmak için toplu test kullanma

Bu öğretici toplu sınama utterance tahmin sorunları bulmak için nasıl kullanılacağını gösterir.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Bir toplu iş test dosyası oluşturma 
* Bir toplu testi çalıştırma
* Test sonuçlarını gözden geçirme
* Hedefleri için hataları giderin
* Toplu işlem yeniden sınayın

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Bu makalede, etmeniz bir [LUIS][LUIS] LUIS uygulamanızı yazmak için hesap.

> [!Tip]
> Bir abonelik zaten yoksa için kaydedebilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

## <a name="create-new-app"></a>Yeni uygulama oluşturma
Bu makalede önceden oluşturulmuş etki alanı HomeAutomation kullanır. Önceden oluşturulmuş etki alanı hedefleri, varlıkları ve ışık gibi HomeAutomation cihazlarını denetleme utterances sahiptir. Uygulama oluşturma, etki alanı ekleme eğitmek ve yayımlayın.

1. İçinde [LUIS] Web sitesi, seçerek yeni bir uygulama oluşturun **yeni uygulama oluştur** üzerinde **MyApps** sayfası. 

    ![Yeni uygulama oluşturma](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Bir ad girin `Batchtest-HomeAutomation` iletişim.

    ![Uygulama adı girin](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Seçin **önceden oluşturulmuş etki alanları** sol alt köşedeki içinde. 

    ![Önceden oluşturulmuş etki alanını seçin](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Seçin **etki alanı ekleme** HomeAutomation için.

    ![HomeAutomation etki alanı ekleme](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Seçin **tren** sağ üst gezinti çubuğunda.

    ![Tren düğmesini seçin](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Toplu test ölçütleri
Toplu sınama aynı anda en fazla 1000 utterances test edebilirsiniz. Toplu işlem yinelenen değer yok. [Dışarı aktarma](create-new-app.md#export-app) geçerli utterances listesini görmek için uygulama.  

LUIS test stratejisi, verilerin üç ayrı kümelerini kullanır: model utterances, toplu test utterances ve uç nokta utterances. Bu öğretici için (bir hedefi eklenir) ya da modeli utterances gelen utterances veya uç nokta utterances kullanmadığınızdan emin olun. 

Utterances hiçbirini zaten uygulamada toplu test için kullanmayın:

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

2. İle utterances ekleme **hedefi** testinde tahmin edilen istiyor. Bu öğreticide, basit hale getirmek üzere utterances olur `HomeAutomation.TurnOn` ve `HomeAutomation.TurnOff` ve geçiş `on` ve `off` utterances metin. İçin `None` amacı, birkaç olmayan utterances ekleme parçası [etki alanı](luis-glossary.md#domain) (konu) alanı. 

    JSON toplu iş için toplu test sonuçlarını nasıl ilişkilendirmek anlamak için yalnızca altı hedefleri ekleyin.

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

## <a name="run-the-batch"></a>Toplu işi çalıştırın
1. Seçin **Test** üst gezinti çubuğunda. 

    ![Gezinti çubuğunda test seçin](./media/luis-tutorial-batch-testing/test-1.png)

2. Seçin **test paneli toplu** sağ taraftaki panelinde. 

    ![Toplu test paneli seçin](./media/luis-tutorial-batch-testing/test-2.png)

3. Seçin **alma dataset**.

    ![İçeri aktarma veri kümesini seçin](./media/luis-tutorial-batch-testing/test-3.png)

4. Dosya sistemi konumunu seçin `homeauto-batch-1.json` dosya.

5. Veri kümesi adı `set 1`.

    ![Dosya seç](./media/luis-tutorial-batch-testing/test-4.png)

6. **Çalıştır** düğmesini seçin. Test tamamlanana kadar bekleyin.

    ![Çalıştır'ı seçin](./media/luis-tutorial-batch-testing/test-5.png)

7. Seçin **bkz sonuçları**.

    ![Sonuçları görmek](./media/luis-tutorial-batch-testing/test-6.png)

8. Gösterge ve grafik sonuçları gözden geçirin.

    ![Toplu iş sonuçları](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Toplu iş sonuçları gözden geçirin
Toplu iş sonuçları iki bölümde olur. Üst kısmında, grafik ve gösterge içerir. Grafik alanı adı seçtiğinizde utterances alt kısmında görüntülenir.

Hataları kırmızı renk tarafından belirtilir. Grafik iki kırmızı olarak görüntülenen bölümlerin dört bölüm kullanılıyor. **Odağı bölümlere üzerinde bunlar**. 

Test bölümü hatalı gösterir sağ üst bir hedefi veya varlık varlığını tahmin. Alt sol bölümü test yanlış bir hedefi veya varlık yokluğu tahmin gösterir.

### <a name="homeautomationturnoff-test-results"></a>HomeAutomation.TurnOff test sonuçları
Açıklamada seçin `HomeAutomation.TurnOff` hedefi. Açıklamada adının solundaki bir yeşil başarı simgesine içeriyor. Bu amaç için hatalar yoktur. 

![Toplu iş sonuçları](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>HomeAutomation.TurnOn ve hiçbiri hedefleri sahip hataları
Diğer iki amacı test tahminleri toplu iş dosyası beklentilerini eşleşmiyordu anlamına gelen hataları var. Seçin `None` ilk hata gözden geçirmek için göstergede hedefi. 

![Hedefi yok](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Hatalar görünür kırmızı bölümlerde grafik: **yanlış pozitif** ve **False negatif**. Seçin **False negatif** grafiğin altındaki başarısız utterances görmek için grafikte bölüm adı. 

![False negatif hataları](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

Başarısız olan utterance `help` olarak bekleniyordu bir `None` hedefi ancak tahmin test `HomeAutomation.TurnOn` hedefi.  

İki hataları, HomeAutomation.TurnOn birinde ve hiçbiri birinde vardır. Her ikisi tarafından utterance kaynaklanan `help` çünkü hiçbiri beklentiyi başarısız oldu ve HomeAutomation.TurnOn amacı için beklenmeyen bir eşleşme oluştu. 

Nedenini belirlemek için `None` utterances başarısız oluyor, halen utterances gözden `None`. 

## <a name="review-none-intents-utterances"></a>Gözden geçirme hedefi hiçbiri utterances kullanıcının

1. Kapat **Test** seçerek paneli **Test** üst gezinti çubuğundaki düğmesini. 

2. Seçin **yapı** üst gezinti panelinden. 

3. Seçin **hiçbiri** hedefleri listesinden hedefi.

4. Utterances belirteci görünümünü görmek için Denetim + E seçin 
    
    |Hedefi hiçbiri utterances kullanıcının|Tahmin puanı|
    |--|--|
    |"Sıcaklık benim için lütfen azaltmak"|0.44|
    |"dim mutfak ışık 25."|0.43|
    |"biriminiz daha düşük"|0.46|
    |"Benim yatak Lütfen Internet Aç"|0.28|

## <a name="fix-none-intents-utterances"></a>Düzeltme yok hedefi utterances kullanıcının
    
Tüm utterances `None` uygulama etki alanı dışındaki olması beklenir. Yanlış hedefi; bu nedenle bu utterances HomeAutomation göre ' dir. 

LUIS ayrıca verir % 50'değerinden küçük utterances (<.50) tahmin puanı. Diğer iki amacı, utterances bakarsanız, çok daha yüksek tahmin puanları bakın. LUIS anlarsınız örnek utterances, düşük puanlarını olduğunda utterances geçerli amacını ve diğer amaçlar arasında için LUIS karmaşıktır. 

Uygulama, halen utterances düzeltmek için `None` hedefi doğru hedefi taşınması gerekir ve `None` hedefi yeni, uygun hedefleri gerekiyor. 

Üç utterances `None` hedefi Otomasyon aygıt ayarları düşürmek için amacı. Sözcükler gibi kullandığınız `dim`, `lower`, veya `decrease`. Internet üzerinde etkinleştirmek dördüncü utterance sorar. Tüm dört utterances açma veya bir aygıta güç derecesini değiştirme hakkında olduğundan, bunlar için taşınması gereken `HomeAutomation.TurnOn` hedefi. 

Bu yalnızca bir çözümdür. Yeni bir amacı da oluşturabilirsiniz `ChangeSetting` dim, kullanarak utterances taşıma azaltmak ve yeni o hedefi azaltmak. 

## <a name="fix-the-app-based-on-batch-results"></a>Toplu sonuçlarına dayalı uygulama düzeltme
Dört utterances taşıma `HomeAutomation.TurnOn` hedefi. 

1. Tüm utterances seçili şekilde utterance listesinin onay kutusunu seçin. 

2. İçinde **yeniden hedefi** açılan listesinde, select `HomeAutomation.TurnOn`. 

    ![Utterances Taşı](./media/luis-tutorial-batch-testing/move-utterances.png)

    Dört utterances atanıncaya sonra utterance listelemek için `None` hedefi boştur.

3. Dört yeni amacı için hiçbiri hedefi ekleyin:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Bu utterances HomeAutomation etki alanının dışında kesinlikle var. Her utterance girerken skoru için izleyin. Puanı düşük veya daha çok düşük (ile etrafında kırmızı bir kutu) olabilir. 8. adımda uygulama eğitmek sonra puanı çok daha yüksek olur. 

7. Mavi etiketi seçin ve utterance içinde seçerek tüm etiketleri Kaldır **Etiketi Kaldır**.

8. Seçin **tren** sağ üst gezinti çubuğunda. Her utterance puanı çok daha yüksektir. Tüm puanlarını `None` hedefi olmalıdır.80 şimdi. 

## <a name="verify-the-fix-worked"></a>Düzeltme çalışılan doğrulayın
İçin toplu testinde utterances doğru şekilde tahmin doğrulamak için **hiçbiri** amacı, toplu test yeniden çalıştırın.

1. Seçin **Test** üst gezinti çubuğunda. 

2. Seçin **test paneli toplu** sağ taraftaki panelinde. 

3. Toplu işlem adı'nın sağındaki üç nokta (...) seçip **çalıştırmak Dataset**. Toplu test tamamlanana kadar bekleyin.

    ![Veri kümesi çalıştırın](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Seçin **bkz sonuçları**. Hedefleri tüm hedefi adları solundaki yeşil simgeler olması gerekir. Kümesine sağ filtreli `HomeAutomation.Turnoff` yeşil nokta grafiği Orta için en yakın üst sağ panelinde hedefini seçin. Grafik aşağıdaki tabloda utterance adı görüntülenir. Puanı `breezeway off please` çok düşük olur. İsteğe bağlı bir etkinlik, bu puanı artırmak yapma için daha fazla utterances eklemektir. 

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

4. If the list of datasets is not visible, select **Back to list**. Select the three dots (...) at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek utterances hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions