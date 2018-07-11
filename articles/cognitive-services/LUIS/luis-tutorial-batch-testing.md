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
ms.date: 07/06/2018
ms.author: v-geberr
ms.openlocfilehash: 962f33a178048c459e8c6c2948eb17f0e78904ae
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930999"
---
# <a name="improve-app-with-batch-test"></a>Toplu test ile uygulama geliştirme

Bu öğreticide, batch test utterance tahmin sorunları bulmak için nasıl kullanılacağı gösterilmektedir.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

<!-- green checkmark -->
> [!div class="checklist"]
* Toplu test dosyası oluşturma 
* Toplu test çalıştırma
* Test sonuçlarını gözden geçirme
* Hedefleri için hataları düzeltin
* Batch yeniden test et

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md#luis-website) hesabına ihtiyacınız olacak.

## <a name="before-you-begin"></a>Başlamadan önce
İnsan Kaynakları uygulamadan yoksa [konuşma uç noktası gözden](luis-tutorial-review-endpoint-utterances.md) öğreticide [alma](luis-how-to-start-new-app.md#import-new-app) JSON'a yeni bir uygulama [LUIS](luis-reference-regions.md#luis-website) Web sitesi. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-review-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `batchtest` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="purpose-of-batch-testing"></a>Toplu test amaçlı
Toplu test test konuşma bilinen bir dizi bir model durumuyla doğrulamanıza olanak sağlar ve varlık etiketli. JSON biçimli bir toplu iş dosyası Konuşma ekleme ve gereksinim duyduğunuz varlık etiketleri içinde utterance tahmin ayarlayın. 

LUIS için önerilen test stratejisinin üç ayrı veri kümelerini kullanır: model, batch test konuşma ve konuşma uç noktası için sağlanan örnek konuşma. Bu öğreticide, konuşma (bir amaç için eklenir) ya da örnek konuşma gelen veya konuşma uç noktası kullanmadığınızdan emin olun. 

Batch test konuşma uç noktası konuşma ve örnek konuşma karşı doğrulamak için [dışarı](luis-how-to-start-new-app.md#export-app) uygulama ve [indirme](luis-how-to-start-new-app.md#export-endpoint-logs) sorgu günlüğü. Sorgu günlüğü konuşma toplu test konuşma ve uygulama örnek utterance'nın karşılaştırın. 

Toplu test etmek için gereksinimler:

* test başına 1000 konuşma. 
* Yinelenen değer yok. 
* İzin verilen varlık türleri: Basit ve bileşik.

## <a name="create-a-batch-file-with-utterances"></a>Konuşma ile bir toplu iş dosyası oluşturma
1. Oluşturma `HumanResources-jobs-batch.json` gibi bir metin düzenleyicisinde [VSCode](https://code.visualstudio.com/). Veya indirme [dosya](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorial-batch-testing/HumanResources-jobs-batch.json) LUIS örnekleri Github deposundan.

2. Konuşma ile JSON biçimli toplu iş dosyasında, ekleme **hedefi** testinde tahmin edilen istiyor. 

    ```JSON
    [
        {
        "text": "Are there any janitorial jobs currently open?",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "I would like a fullstack typescript programming with azure job",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "Is there a database position open in Los Colinas?",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "Can I apply for any database jobs with this resume?",
        "intent": "GetJobInformation",
        "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>Toplu işlem çalıştırın

1. Seçin **Test** üst gezinti çubuğunda. 

    [ ![Üst, sağ gezinti çubuğunda vurgulanmış Test ile ekran görüntüsü, LUIS uygulaması](./media/luis-tutorial-batch-testing/hr-first-image.png)](./media/luis-tutorial-batch-testing/hr-first-image.png#lightbox)

2. Seçin **test paneli toplu** Sağdaki panelde. 

    [ ![Toplu test paneliyle vurgulanmış ekran görüntüsü, LUIS uygulama](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Seçin **alma dataset**.

    [ ![Vurgulanmış içeri aktarma veri kümesi ile ekran görüntüsü, LUIS uygulaması](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. Dosya sistemi konumunu seçin `HumanResources-jobs-batch.json` dosya.

5. Veri kümesi adı `intents only` seçip **Bitti**.

    ![Dosya seç](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. **Çalıştır** düğmesini seçin. Test işlemi tamamlanana kadar bekleyin.

    [ ![Vurgulanan çalışma ile ekran görüntüsü, LUIS uygulaması](./media/luis-tutorial-batch-testing/hr-run-button.png)](./media/luis-tutorial-batch-testing/hr-run-button.png#lightbox)

7. Seçin **bkz sonuçları**.

8. Gösterge ve grafik sonuçlarını gözden geçirin.

    [ ![Toplu test sonuçları ile ekran görüntüsü, LUIS uygulaması](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Toplu iş sonuçlarını gözden geçirin
Batch grafik dört quadrants sonuçlarını görüntüler. Grafiğin sağına bir filtredir. Varsayılan olarak, listedeki ilk amaca filtre ayarlanır. Tüm amaçlar ve yalnızca basit, hiyerarşik filtre içerir (üst salt) ve bileşik varlıkları. Bir bölümü veya grafik içinde bir nokta seçtiğinizde, ilişkili utterance(s) grafiğin altına görüntüler. 

Grafik üzerine gelindiğinde, fare tekerleğini büyütebilir veya grafikte görüntülenecek azaltın. Bu, sıkı bir şekilde birlikte kümelenmiş grafik üzerinde çok sayıda noktası olduğunda yararlıdır. 

Grafik dört Çeyrek dairelerle iki kırmızı renkte gösterilir bölümlerin birlikte kullanılıyor. **Temel odak noktası bölümlere bunlar**. 

### <a name="applyforjob-test-results"></a>ApplyForJob test sonuçları
**ApplyForJob** filtrede görüntülenen test sonuçları göster 1 dört tahminlerin başarılı oldu. Adı seçin **hatalı pozitif sonuç** grafiğin altındaki konuşma görmek için sağ üst quadrant üstünde. 

![LUIS toplu test konuşma](./media/luis-tutorial-batch-testing/hr-applyforjobs-false-positive-results.png)

Üç konuşma üst amacı olan **ApplyForJob**. Toplu iş dosyasında belirtilen hedefi daha düşük bir puan vardı. Neden bu ortaya çıktı? İki amacı, word seçeneği ve word düzenleme bakımından çok yakından ilişkilidir. Ayrıca, neredeyse üç kez daha fazla örnek için vardır **ApplyForJob** daha **GetJobInformation**. Bu düz olmayan örnek konuşma, ağırlıklandıran **ApplyForJob** amaç'ın ayrıcalık. 

Her iki amacı aynı hataların sayısını olduğunu fark edeceksiniz: 

![LUIS toplu test filtre hataları](./media/luis-tutorial-batch-testing/hr-intent-error-count.png)

Üst noktanın içinde karşılık gelen utterance **hatalı pozitif sonuç** bölüm `Can I apply for any database jobs with this resume?`. Word `resume` yalnızca içinde kullanılan **ApplyForJob**. 

Diğer iki nokta grafikte doğru ıntent'e daha yakından olduklarından yani yanlış hedefi için çok daha düşük puanlar vardı. 

## <a name="fix-the-app-based-on-batch-results"></a>Batch sonuçlarına göre uygulama düzeltme
Bu bölümün amacı, yanlış için tahmin üç konuşma sağlamaktır **ApplyForJob** için doğru şekilde tahmin için **GetJobInformation**, uygulamayı düzeltildikten sonra. 

Bu toplu dosya konuşma doğru ıntent'e ekleme görünüşte hızlı düzeltme olacaktır. Yine de yapmak istediğinizi değil olmasıdır. LUIS, örnek olarak eklemeden Bu konuşma doğru şekilde tahmin etmek istediğiniz. 

Konuşma alanından kaldırma hakkında da merak edebilirsiniz **ApplyForJob** utterance miktarı aynı olana kadar **GetJobInformation**. Test sonuçlarını giderebilir ancak bu hedefi doğru sonraki tahmin gelen LUIS azaltabilir. 

Daha fazla konuşma eklemek için ilk düzeltmesidir **GetJobInformation**. Word ağırlığını azaltmak için ikinci düzeltmesidir `resume` doğru **ApplyForJob** hedefi. 

### <a name="add-more-utterances-to-getjobinformation"></a>Daha fazla konuşma için ekleme **GetJobInformation**
1. Toplu test paneli seçerek kapatmak **Test** üst gezinti panelinde düğmesini. 

    [ ![Test düğmesi vurgulanan LUIS ekran görüntüsü](./media/luis-tutorial-batch-testing/hr-close-test-panel.png)](./media/luis-tutorial-batch-testing/hr-close-test-panel.png#lightbox)

2. Seçin **GetJobInformation** hedefleri listesinde. 

    [ ![Test düğmesi vurgulanan LUIS ekran görüntüsü](./media/luis-tutorial-batch-testing/hr-select-intent-to-fix-1.png)](./media/luis-tutorial-batch-testing/hr-select-intent-to-fix-1.png#lightbox)

3. Uzunluğu, word seçeneği ve word düzenleme, koşulları eklediğinizden emin olmak için değiştirilen daha fazla Konuşma ekleme `resume` ve `c.v.`:

    ```JSON
    Is there a new job in the warehouse for a stocker?
    Where are the roofing jobs today?
    I heard there was a medical coding job that requires a resume.
    I would like a job helping college kids write their c.v.s. 
    Here is my resume, looking for a new post at the community college using computers.
    What positions are available in child and home care?
    Is there an intern desk at the newspaper?
    My C.v. shows I'm good at analyzing procurement, budgets, and lost money. Is there anything for this type of work?
    Where are the earth drilling jobs right now?
    I've worked 8 years as an EMS driver. Any new jobs?
    New food handling jobs?
    How many new yard work jobs are available?
    Is there a new HR post for labor relations and negotiations?
    I have a masters in library and archive management. Any new positions?
    Are there any babysitting jobs for 13 year olds in the city today?
    ```

4. Seçerek uygulama eğitme **eğitme** üst sağ gezinti.

## <a name="verify-the-fix-worked"></a>Düzeltme çalışılan doğrulayın
Toplu test konuşma doğru şekilde tahmin doğrulamak için batch testi yeniden çalıştırın.

1. Seçin **Test** üst gezinti çubuğunda. Toplu sonuçları hala açıksa seçin **listesine geri**.  

2. Öğesinin üç noktasını (***...*** ) sağında toplu işlem adı'düğmesine tıklayın ve belirleyin **çalıştırma Dataset**. Toplu test işlemi tamamlanana kadar bekleyin. Dikkat **bkz sonuçları** düğme yeşildir şimdi. Başka bir deyişle, toplu işin tamamını başarıyla çalıştırıldı.

3. Seçin **bkz sonuçları**. Intents tüm hedefi adları solundaki yeşil simgeler olması gerekir. 

    [ ![Toplu sonuçları düğmesi vurgulanan LUIS ekran görüntüsü](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png)](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png#lightbox)


## <a name="what-has-this-tutorial-accomplished"></a>Bu öğreticide nelerin?
Bu uygulama tahmin doğruluğunu toplu işlemde hataları bulma ve model doğru amacı ve eğitim için daha fazla örnek konuşma ekleyerek düzeltme arttı. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Seçin **uygulamalarım** üstteki soldaki menüde. Noktayı **...**  sağında bulunan uygulama listesinde uygulama adı, seçin **Sil**. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Desenler hakkında bilgi edinin](luis-tutorial-pattern.md)

