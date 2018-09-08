---
title: LUIS tahminleri artırmak için toplu test kullanma | Microsoft Docs
titleSuffix: Azure
description: Yük toplu test sonuçlarını gözden geçirin ve değişikliklerle LUIS tahminleri geliştirmek.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 5abaeaee87d54e82df29e75b89c83522b8746730
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158254"
---
# <a name="improve-app-with-batch-test"></a>Toplu test ile uygulama geliştirme

Bu öğreticide, batch test utterance tahmin sorunları bulmak için nasıl kullanılacağı gösterilmektedir.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

<!-- green checkmark -->
> [!div class="checklist"]
* Toplu test dosyası oluşturma 
* Toplu test çalıştırma
* Test sonuçlarını gözden geçirme
* Hataları düzelt 
* Batch yeniden test et

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Başlamadan önce

İnsan Kaynakları uygulamadan yoksa [konuşma uç noktası gözden](luis-tutorial-review-endpoint-utterances.md) öğreticide [alma](luis-how-to-start-new-app.md#import-new-app) JSON'a yeni bir uygulama [LUIS](luis-reference-regions.md#luis-website) Web sitesi. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-review-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `batchtest` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

Uygulama eğitin.

## <a name="purpose-of-batch-testing"></a>Toplu test amaçlı

Toplu test etkin doğrulamanıza olanak tanır, modelin bilinen durumuyla etiketli konuşma ve varlıkların eğitim. JSON biçimli bir toplu iş dosyası Konuşma ekleme ve gereksinim duyduğunuz varlık etiketleri içinde utterance tahmin ayarlayın. 

<!--The recommended test strategy for LUIS uses three separate sets of data: example utterances provided to the model, batch test utterances, and endpoint utterances. --> Bu öğreticinin dışında bir uygulama kullanırken, emin olun *değil* zaten bir amaç için eklenen örnek konuşma kullanarak. Batch test konuşma örnek konuşma karşı doğrulamak için [dışarı](luis-how-to-start-new-app.md#export-app) uygulama. Uygulama örnek utterance'nın batch test konuşma için karşılaştırın. 

Toplu test etmek için gereksinimler:

* En fazla test başına 1000 konuşma. 
* Yinelenen değer yok. 
* İzin verilen varlık türleri: basit, hiyerarşik yalnızca Eve öğrenilen varlıklar (salt üst) ve karma. Toplu test yalnızca öğrenilen Eve amaç ve varlıkları için yararlı olur.

## <a name="create-a-batch-file-with-utterances"></a>Konuşma ile bir toplu iş dosyası oluşturma

1. Oluşturma `HumanResources-jobs-batch.json` gibi bir metin düzenleyicisinde [VSCode](https://code.visualstudio.com/). 

2. Konuşma ile JSON biçimli toplu iş dosyasında, ekleme **hedefi** testinde tahmin edilen istiyor. 

   [!code-json[Add the intents to the batch test file](~/samples-luis/documentation-samples/tutorial-batch-testing/HumanResources-jobs-batch.json "Add the intents to the batch test file")]

## <a name="run-the-batch"></a>Toplu işlem çalıştırın

1. Seçin **Test** üst gezinti çubuğunda. 

2. Seçin **test paneli toplu** Sağdaki panelde. 

    [![Toplu test paneliyle vurgulanmış ekran görüntüsü, LUIS uygulama](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Seçin **alma dataset**.

    [![Vurgulanmış içeri aktarma veri kümesi ile ekran görüntüsü, LUIS uygulaması](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. Dosya sistemi konumunu seçin `HumanResources-jobs-batch.json` dosya.

5. Veri kümesi adı `intents only` seçip **Bitti**.

    ![Dosya seç](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. **Çalıştır** düğmesini seçin. Test işlemi tamamlanana kadar bekleyin.

7. Seçin **bkz sonuçları**.

8. Gösterge ve grafik sonuçlarını gözden geçirin.

    [![Toplu test sonuçları ile ekran görüntüsü, LUIS uygulaması](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Toplu iş sonuçlarını gözden geçirin

Batch grafik dört quadrants sonuçlarını görüntüler. Grafiğin sağına bir filtredir. Varsayılan olarak, listedeki ilk amaca filtre ayarlanır. Tüm amaçlar ve yalnızca basit, hiyerarşik filtre içerir (üst salt) ve bileşik varlıkları. Seçtiğinizde, bir [grafik bölümünü](luis-concept-batch-test.md#batch-test-results) veya bir nokta grafik içinde ilişkili utterance(s) grafiğin altına görüntüleyebilirsiniz. 

Grafik üzerine gelindiğinde, fare tekerleğini büyütebilir veya grafikte görüntülenecek azaltın. Bu, sıkı bir şekilde birlikte kümelenmiş grafik üzerinde çok sayıda noktası olduğunda yararlıdır. 

Grafik dört Çeyrek dairelerle iki kırmızı renkte gösterilir bölümlerin birlikte kullanılıyor. **Temel odak noktası bölümlere bunlar**. 

### <a name="getjobinformation-test-results"></a>GetJobInformation test sonuçları

**GetJobInformation** filtrede görüntülenen test sonuçları göster 2 dört tahminlerin başarılı. Adı seçin **hatalı pozitif sonuç** grafiğin altındaki konuşma görmek için sağ üst quadrant üstünde. 

![LUIS toplu test konuşma](./media/luis-tutorial-batch-testing/hr-applyforjobs-false-positive-results.png)

Neden olarak tahmin konuşma ikisidir **ApplyForJob**, doğru amacı yerine **GetJobInformation**? İki amacı, word seçeneği ve word düzenleme bakımından çok yakından ilişkilidir. Ayrıca, neredeyse üç kez daha fazla örnek için vardır **ApplyForJob** daha **GetJobInformation**. Bu düz olmayan örnek konuşma, ağırlıklandıran **ApplyForJob** amaç'ın ayrıcalık. 

Her iki amacı aynı hataların sayısını olduğunu fark edeceksiniz. Tek amacı, yanlış bir tahmin diğer hedefi de etkiler. Konuşma yanlış bir amaç için tahmin edilen ve başka bir amaç için Ayrıca yanlış tahmin değil çünkü her ikisi de hatalarla karşılaştınız. 

![LUIS toplu test filtre hataları](./media/luis-tutorial-batch-testing/hr-intent-error-count.png)

Üst karşılık gelen sesleri nokta **hatalı pozitif sonuç** bölümü olan `Can I apply for any database jobs with this resume?` ve `Can I apply for any database jobs with this resume?`. İlk utterance, word için `resume` yalnızca içinde kullanılan **ApplyForJob**. İkinci utterance, word için `apply` yalnızca içinde kullanılan **ApplyForJob** hedefi.

## <a name="fix-the-app-based-on-batch-results"></a>Batch sonuçlarına göre uygulama düzeltme

Bu bölümde tüm sesleri doğru şekilde tahmin için hedefidir **GetJobInformation** uygulama düzeltme tarafından. 

Bu toplu dosya konuşma doğru ıntent'e ekleme görünüşte hızlı düzeltme olacaktır. Yine de yapmak istediğinizi değil olmasıdır. LUIS, örnek olarak eklemeden Bu konuşma doğru şekilde tahmin etmek istediğiniz. 

Konuşma alanından kaldırma hakkında da merak edebilirsiniz **ApplyForJob** utterance miktarı aynı olana kadar **GetJobInformation**. Test sonuçlarını giderebilir ancak bu hedefi doğru sonraki tahmin gelen LUIS azaltabilir. 

Daha fazla konuşma eklemek için ilk düzeltmesidir **GetJobInformation**. İkinci düzeltme gibi bir kelimelerin ağırlık azaltmaktır `resume` ve `apply` doğru **ApplyForJob** hedefi. 

### <a name="add-more-utterances-to-getjobinformation"></a>Daha fazla konuşma için ekleme **GetJobInformation**

1. Toplu test paneli seçerek kapatmak **Test** üst gezinti panelinde düğmesini. 

2. Seçin **GetJobInformation** hedefleri listesinde. 

3. Uzunluğu, word seçeneği ve word düzenleme, koşulları eklediğinizden emin olmak için değiştirilen daha fazla Konuşma ekleme `resume`, `c.v.`, ve `apply`:

    |İçin örnek konuşma **GetJobInformation** hedefi|
    |--|
    |Yeni bir stocker ambarı işinde bir özgeçmiş uygulayabilirim gerektiriyor mu?|
    |Bugün çatı işleri nerede?|
    |Ben bir özgeçmiş gerektiren Tıbbi bir kodlama işi oluştu olduğunu.|
    |Üniversite Çocuklar kendi c.v.s. yazma yardımcı olan bir işi istiyorum |
    |Bilgisayarları kullanarak yüksekokul, yeni bir gönderi mi arıyorsunuz my sürdürme aşağıda verilmiştir.|
    |Hangi konumları alt ve giriş dikkatli kullanılabilir mi?|
    |Stajyeri Masası gazete sırasında mı?|
    |My C.v. satın alma, bütçe ve kayıp para çözümleme en iyi ben gösterir. Bu türdeki bir iş için bir şey var mı?|
    |Burada dünya işleri şu anda ayrıntılara misiniz?|
    |Ben 8 yıl EMS sürücü olarak çalıştım. Tüm yeni işler?|
    |Yeni Gıda işleme işleri uygulama gerektiriyor?|
    |Kaç yeni yard iş işleri kullanılabilir mi?|
    |Yeni bir SA gönderi işçilik ilişkileri ve anlaşmaları var mı?|
    |Kitaplık ve Arşiv Yönetimi'nde bir asıl var. Tüm yeni konumlarına?|
    |Babysitting tüm işler için 13 yıl grubundakiler şehirde bugün var mı?|

    Etiket yok **iş** konuşma varlık. Öğreticinin bu bölümünde, yalnızca hedefi tahmin odaklanmıştır.

4. Seçerek uygulama eğitme **eğitme** üst sağ gezinti.

## <a name="verify-the-fix-worked"></a>Düzeltme çalışılan doğrulayın

Toplu test konuşma doğru şekilde tahmin doğrulamak için batch testi yeniden çalıştırın.

1. Seçin **Test** üst gezinti çubuğunda. Toplu sonuçları hala açıksa seçin **listesine geri**.  

2. Öğesinin üç noktasını (***...*** ) sağında toplu işlem adı'düğmesine tıklayın ve belirleyin **çalıştırma Dataset**. Toplu test işlemi tamamlanana kadar bekleyin. Dikkat **bkz sonuçları** düğme yeşildir şimdi. Başka bir deyişle, toplu işin tamamını başarıyla çalıştırıldı.

3. Seçin **bkz sonuçları**. Intents tüm hedefi adları solundaki yeşil simgeler olması gerekir. 

    ![Toplu sonuçları düğmesi vurgulanan LUIS ekran görüntüsü](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png)

## <a name="create-batch-file-with-entities"></a>Toplu iş dosyası ile varlıkları oluşturun 

Toplu test varlıklarda doğrulamak için varlıkları batch JSON dosyasında etiketlenmesi gerekir. Yalnızca makine öğrenilen varlıklar kullanılır: basit, hiyerarşik (ana salt) ve bileşik varlıkları. Makine öğrenilen varlıklar her zaman normal ifadeler üzerinden bulundukları veya açık metinle eşleşen eklemeyin.

Varlıklar için toplam word çeşitlemesi ([belirteci](luis-glossary.md#token)) sayısı, tahmin kalite etkileyebilir. Varlık uzunluklarının çeşitli amaca etiketli Konuşma ile sağlanan eğitim verilerini içerdiğinden emin olun. 

İlk yazma ve toplu iş dosyaları test, birkaç Konuşma ile başlatmak en iyi ve bildiğiniz varlıkları çalışma yanı sıra, birkaç, düşünme yanlış tahmin. Bu, içinde sorun alanlara hızlıca odaklanmak yardımcı olur. Test sonra **GetJobInformation** ve **ApplyForJob** amacı, tahmin değil, birkaç farklı iş adları kullanarak tahmin ile ilgili bir sorun olup olmadığını görmek için bu toplu test dosyası geliştirildi için belirli değerlerle **iş** varlık. 

Değerini bir **iş** test konuşma içinde sağlanan varlıktır genellikle daha fazla sözcük olan birkaç örnekleri içeren bir veya iki sözcük. Varsa _kendi_ İnsan Kaynakları uygulamasında genellikle birçok bir kelimelerin iş adları varsa, örnek Konuşma ile etiketlenmiş **iş** bu uygulamada varlık iyi çalışmamasına.

1. Oluşturma `HumanResources-entities-batch.json` gibi bir metin düzenleyicisinde [VSCode](https://code.visualstudio.com/). Veya indirme [dosya](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorial-batch-testing/HumanResources-entities-batch.json) LUIS örnekleri Github deposundan.


2. JSON biçimli bir toplu iş dosyasında bir konuşma ile içeren bir nesne dizisi Ekle **hedefi** utterance herhangi bir varlık konumlarını yanı sıra test tahmin edilen istiyor. Bir varlık belirteç tabanlı olduğundan, her varlık, bir karakter durdurmak ve başlatmak emin olun. Başlayamaz veya bitemez utterance bir alan üzerinde. Bu, toplu iş dosyasını içeri aktarma sırasında bir hataya neden olur.  

   [!code-json[Add the intents and entities to the batch test file](~/samples-luis/documentation-samples/tutorial-batch-testing/HumanResources-entities-batch.json "Add the intents and entities to the batch test file")]


## <a name="run-the-batch-with-entities"></a>Batch ile varlıkları çalıştırma

1. Seçin **Test** üst gezinti çubuğunda. 

2. Seçin **test paneli toplu** Sağdaki panelde. 

3. Seçin **alma dataset**.

4. Dosya sistemi konumunu seçin `HumanResources-entities-batch.json` dosya.

5. Veri kümesi adı `entities` seçip **Bitti**.

6. **Çalıştır** düğmesini seçin. Test işlemi tamamlanana kadar bekleyin.

7. Seçin **bkz sonuçları**.

## <a name="review-entity-batch-results"></a>Varlık toplu sonuçları gözden geçirin

Grafik doğru şekilde tahmin edilen tüm hedefleri ile açılır. Varlık Öngörüler erroring bulmak için sağ taraftaki filtreye kaydırın. 

1. Seçin **iş** filtredeki varlık.

    ![Filtredeki erroring varlık Öngörüler](./media/luis-tutorial-batch-testing/hr-entities-filter-errors.png)

    Varlık Öngörüler görüntülemek için grafik değişir. 

2. Seçin **False negatif** quadrant grafiğin alt, sol. Ardından klavye birleşimi control + E belirteci görünümüne geçiş yapmak için kullanın. 

    [![Varlık Öngörüler belirteci görünümü](./media/luis-tutorial-batch-testing/token-view-entities.png)](./media/luis-tutorial-batch-testing/token-view-entities.png#lightbox)
    
    Grafiğin altındaki konuşma gözden geçirme ortaya çıkarır tutarlı bir hata iş adı içerdiğinde `SQL`. Örnek konuşma ve iş ifade listesi gözden geçirme, SQL yalnızca bir kez kullanılır ve yalnızca büyük bir iş adı bir parçası olarak olduğu `sql/oracle database administrator`.

## <a name="fix-the-app-based-on-entity-batch-results"></a>Varlık toplu sonuçlarına göre uygulama düzeltme

Uygulama düzeltme SQL işleri çeşitleri doğru şekilde belirlemek LUIS gerektirir. Bu düzeltme için birkaç seçenek vardır. 

* Açıkça SQL kullanın ve bir iş varlığı sözcükleri etiket daha fazla örnek konuşma ekleyin. 
* Açıkça daha fazla SQL işleri ifade listesine ekleyin

Bu görevleri sizin için yapmasını bırakılır.

Ekleme bir [deseni](luis-concept-patterns.md) varlık doğru şekilde tahmin önce sorunu gidermek için gittiği değil. Desen deseninde tüm varlıkları algılanan kadar eşleşmeyecektir olmasıdır. 

## <a name="what-has-this-tutorial-accomplished"></a>Bu öğretici hangi işlemleri gerçekleştirdi?

Uygulama tahmin doğruluğunu toplu işlemde hataları bulma ve düzeltme model arttı. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Desenler hakkında bilgi edinin](luis-tutorial-pattern.md)

