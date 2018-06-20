---
title: Toplu test HALUK uygulamanızı - Azure | Microsoft Docs
description: Dil anlama (HALUK) toplu sınama utterances yanlış hedefleri ve varlıkları bulmak için kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 822fb1e2d5b13941527d242e8501b423bd6b81cb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265522"
---
# <a name="batch-testing"></a>Toplu test etme
 Toplu test HALUK kendi performansını ölçmek için geçerli, eğitilen model üzerinde kapsamlı bir test etmektir. 

<a name="batch-testing"></a>
## <a name="import-a-dataset-file-for-batch-testing"></a>Toplu test etmek için bir veri kümesi dosyasını içeri aktarma

1. Seçin **Test** çubuğu ve ardından üst **test paneli toplu**.

    ![Toplu bağlantısını test etme](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Seçin **alma dataset**. **Alma yeni veri kümesi** iletişim kutusu görüntülenir. Seçin **Dosya Seç** ve bulun [JSON](luis-concept-batch-test.md#batch-file-format) içeren dosya *en çok 1.000* test etmek için utterances.

    ![Veri kümesi dosyasını içeri aktarma](./media/luis-how-to-batch-test/batchtest-importset.png)

    Alma hataları tarayıcı üstündeki kırmızı bildirim çubuğu bildirilir. Alma hataları olduğunda, veri kümesi oluşturulur. Daha fazla bilgi için bkz: [sık karşılaşılan](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. İçinde **veri kümesi adı** alan, veri kümesi dosyanız için bir ad girin. Veri kümesi dosyasını içeren bir **utterances dizisi** dahil olmak üzere *hedefi etiketli* ve *varlıklar*. Gözden geçirme [örnek toplu iş dosyası](luis-concept-batch-test.md#batch-file-format) sözdizimi için. 

4. Seçin **Bitti**. Veri kümesi dosyası eklenir.

## <a name="run-rename-export-or-delete-dataset"></a>Çalıştırmak, yeniden adlandırmak, dışarı veya veri kümesini silmek
Çalıştırmak, yeniden adlandırmak, dışarı veya veri kümesini silmek için üç nokta kullanın (**...** ) dataset satırın sonundaki.

![Veri kümesi Eylemler](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Toplu test eğitilen uygulamanızı çalıştırma

Testi çalıştırmak için veri kümesi adı seçin. Test tamamlandığında bu satır kümesi test sonucunu görüntüler.

![Toplu sınama sonucu](./media/luis-how-to-batch-test/run-test.png)

İndirilebilir dataset toplu test etmek için yüklenen aynı dosyasıdır.

|Durum|Anlamı|
|--|--|
|![Başarılı test yeşil daire simgesi](./media/luis-how-to-batch-test/batch-test-result-green.png)|Tüm utterances başarılı olur.|
|![Başarısız olan test x simgesi kırmızı](./media/luis-how-to-batch-test/batch-test-result-red.png)|En az bir utterance hedefi tahmin eşleşmedi.|
|![Test simgesi için hazır](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Test çalışmaya hazırdır.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>
## <a name="view-batch-test-results"></a>Toplu test sonuçlarını görüntüleme 
Toplu test sonuçlarını gözden geçirmek için seçin **bkz sonuçları**.

![Toplu test sonuçları](./media/luis-how-to-batch-test/run-test-results.png)

<!--
 Select the **See results** link that appears after you run the test. A scatter graph known as an error matrix displays. The data points represent the utterances in the dataset. 

Green points indicate correct prediction, and red ones indicate incorrect prediction.

The filtering panel on the right side of the screen displays a list of all intents and entities in the app, with a green point for intents/entities that were predicted correctly in all dataset utterances, and a red point for those items with errors. Also, for each intent/entity, you can see the number of correct predictions out of the total utterances.

-->


<a name="filter-chart-results-by-intent-or-entity"></a> ## Grafik sonuçları filtresi

Belirli amaç veya varlık grafik filtre uygulamak için sağ taraftaki filtreleme panelinde amacını veya varlık seçin. Veri noktalarını ve bunların dağıtım yaptığınız seçime göre grafikteki güncelleştirin. 
 
![Görselleştirilmiş toplu sınama sonucu](./media/luis-how-to-batch-test/filter-by-entity.png) 

<!--
## Investigate false sections
Data points on the **[False Positive][false-positive]** and **[False Negative][false-negative]** sections indicate errors, which should be investigated. If all data points are on the **[True Positive][true-positive]** and **[True Negative][true-negative]** sections, then your application's performance is perfect on this dataset.


The graph indicates [F-measure][f-measure], [recall][recall], and [precision][precision].  
-->
## <a name="view-single-point-utterance-data"></a>Tek nokta utterance verileri görüntüleme
Grafikte, kendi tahmin çalışılarak puanı görmek için veri noktası üzerinde gelin. Sayfanın altındaki utterances listesinde karşılık gelen kendi utterance almak için bir veri noktasını seçin. 

![Seçili utterance](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="view-section-data"></a>Bölüm verileri görüntüleme
Bölüm adı gibi dört bölüm grafikte seçin **yanlış pozitif** grafiğin sağ üst adresindeki. Aşağıdaki grafik, bu bölümdeki tüm utterances listesinde grafiğin altındaki görüntüler. 

![Seçili utterances bölüme göre](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

Bu önceki görüntüde utterance `switch on` TurnAllOn amacıyla etiketli ancak hedefi hiçbiri tahmin alındı. Bu TurnAllOn hedefi beklenen tahminde bulunmak için daha fazla örnek utterances gerekir göstergesidir. 

Kırmızı grafikte iki bölümden beklenen tahmini eşleşmedi utterances gösterir. Bunlar utterances gösterir hangi HALUK daha fazla eğitim gerekiyor. 

Yeşil grafikte iki bölümden beklenen tahmini eşleşmiyor.

## <a name="next-steps"></a>Sonraki adımlar

Sınama HALUK uygulamanızın doğru hedefleri ve varlıkları tanımıyor gösteriyorsa, daha fazla utterances etiketleme ya da özellikler ekleyerek HALUK uygulamanızın performansını artırmak için çalışabilir. 

* [Önerilen utterances HALUK ile etiket](Label-Suggested-Utterances.md) 
* [HALUK uygulamanızın performansını artırmak için özelliklerini kullanma](luis-how-to-add-features.md) 
* [Bu öğretici ile test toplu anlama](luis-tutorial-batch-testing.md)
* [Kavramları sınama toplu bilgi](luis-concept-batch-test.md).

[true-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-positive
[true-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-negative
[false-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-positive
[false-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-negative
[f-measure]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#f-measure
[recall]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#recall
[precision]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#precision

