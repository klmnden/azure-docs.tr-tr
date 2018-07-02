---
title: 'Öğretici: Yaklaşım analizi döndüren bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide LUIS uygulamanıza konuşmalardaki pozitif, negatif ve nötr duyguları çözümleme amacıyla duygu analizi eklemeyi öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: v-geberr
ms.openlocfilehash: ac959989dbe64460025bfba84df7b6f22c3c1c04
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36958438"
---
# <a name="tutorial-create-app-that-returns-sentiment-along-with-intent-prediction"></a>Yaklaşım ve amaç tahmini döndüren bir uygulama oluşturma
Bu öğreticide konuşmalardaki pozitif, negatif ve nötr yaklaşımları ayıklamayı gösteren bir uygulama oluşturacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Yaklaşım analizini anlama
> * LUIS uygulamasını İnsan Kaynakları (İK) alanında kullanma 
> * Yaklaşım analizi ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama 

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="before-you-begin"></a>Başlamadan önce
[keyPhrase varlıkları](luis-quickstart-intent-and-key-phrase.md) öğreticisinde oluşturulan İnsan Kaynakları uygulamasına sahip değilseniz JSON verilerini [içe aktararak](create-new-app.md#import-new-app) [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulama oluşturun. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-keyphrase-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `sentiment` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="sentiment-analysis"></a>Yaklaşım analizi
Yaklaşım analizi, bir kullanıcının konuşmasının pozitif, negatif veya nötr olma durumunu belirleme yeteneğidir. 

Aşağıdaki konuşmalarda yaklaşım örnekleri gösterilmektedir:

|Yaklaşım|Puan|Konuşma|
|:--|:--|:--|
|pozitif|0,91 |John W. Smith did a great job on the presentation in Paris. (John W. Smith, Paris'teki sunumda harika bir iş çıkardı.)|
|pozitif|0,84 |jill-jones@mycompany.com did fabulous work on the Parker sales pitch. (Kullanıcı Parker satış sunumunda harikaydı.)|

Yaklaşım analizi, tüm konuşmalar için geçerli olan bir uygulama ayarıdır. Yaklaşım analizi konuşmanın tamamını sağladığından konuşmada yaklaşım belirten kelimeleri bulup etiketlemeniz gerekmez. 

## <a name="add-employeefeedback-intent"></a>EmployeeFeedback amacı ekleme 
Şirket üyelerinin çalışanlar hakkındaki geri bildirimini yakalamak için yeni bir amaç ekleyin. 

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

    [ ![Sağ taraftaki menü çubuğunun en üstünde bulunan Build (Derleme) ifadesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png#lightbox)

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin.

    [ ![Sağ taraftaki menü çubuğunun en üstünde bulunan Build (Derleme) ifadesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png#lightbox)

3. Yeni amaca `EmployeeFeedback` adını verin.

    ![EmployeeFeedback adı görünen Create new intent (Yeni amaç oluştur) iletişim kutusu](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Bir çalışanın iyi yaptığı bir işi veya geliştirilmesi gereken bir alanı belirten birden fazla konuşma ekleyin:

    Bu İnsan Kaynakları uygulamasında çalışanların liste varlığı, `Employee`, ad, e-posta, dahili telefon, cep telefonu numarası ve ABD sosyal güvenlik numarası ile tanımlandığını unutmayın. 

    |Konuşmalar|
    |--|
    |425-555-1212 did a nice job of welcoming back a co-worker from maternity leave (425-555-1212, doğum izninden dönen bir çalışma arkadaşı için karşılama düzenlemekle çok iyi yaptı)|
    |234-56-7891 did a great job of comforting a co-worker in their time of grief. (234-56-7891, zor durumda olan bir çalışma arkadaşına destek olmakla iyi yaptı.)|
    |jill-jones@mycompany.com didn't have all the required invoices for the paperwork. (Çalışan, evraklar için gerekli olan tüm faturalara sahip değildi.)|
    |john.w.smith@mycompany.com turned in the required forms a month late with no signatures (Çalışan gerekli formları bir ay gecikmeyle ve imzasız olarak getirdi)|
    |x23456 didn't make it to the important marketing off-site meeting. (x23456 şirket dışındaki önemli pazarlama toplantısına katılmadı.)|
    |x12345 missed the meeting for June reviews. (x12345, Haziran gözden geçirme toplantısını kaçırdı.)|
    |Jill Jones rocked the sales pitch at Harvard (Jill Jones, Harvard'daki satış sunumunda parladı)|
    |John W. Smith did a great job on the presentation at Stanford. (John W. Smith, Stanford'daki sunumda harika bir iş çıkardı.)|

    [ ![EmployeeFeedback amacındaki örnek konuşmaları gösteren LUIS uygulamasının ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar yeni amaç ve örnek konuşmalar hakkında bilgi sahibi değildir. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Vurgulanmış Train (Eğitim) düğmesinin ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı bildirim çubuğunun ekran görüntüsü ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-trained-inline.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>Uygulamayı yaklaşım analizi içerecek şekilde yapılandırma
Yaklaşım analizini **Publish** (Yayımla) sayfasında yapılandırın. 

1. Sağ üst gezinti bölmesinde **Publish** (Yayımla) öğesini seçin.

    ![Publish (Yayımla) düğmesi genişletilmiş Intent (Amaç) sayfasının ekran görüntüsü ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-button-in-top-nav-highlighted.png)

2. **Enable Sentiment Analysis** (Yaklaşım Analizini Etkinleştir) öğesini seçin. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-to-production-expanded.png "Publish to production slot (Üretim yuvasında yayımla) düğmesi vurgulanmış Yayımlama sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-to-production-expanded.png#lightbox)

4. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-an-utterance"></a>Uç noktayı bir konuşmayla sorgulama

1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    ![Uç nokta URL'si vurgulanmış Publish (Yayımla) sayfası ekran görüntüsü](media/luis-quickstart-intent-and-sentiment-analysis/hr-endpoint-url-inline.png)

2. Adres çubuğundaki URL'nin sonuna gidip `Jill Jones work with the media team on the public portal was amazing` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `EmployeeFeedback` amacını yaklaşım analizi ayıklanmış şekilde döndürmelidir.

```
{
  "query": "Jill Jones work with the media team on the public portal was amazing",
  "topScoringIntent": {
    "intent": "EmployeeFeedback",
    "score": 0.4983256
  },
  "intents": [
    {
      "intent": "EmployeeFeedback",
      "score": 0.4983256
    },
    {
      "intent": "MoveEmployee",
      "score": 0.06617523
    },
    {
      "intent": "GetJobInformation",
      "score": 0.04631853
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0103248553
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.007531875
    },
    {
      "intent": "FindForm",
      "score": 0.00344597152
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00337914471
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0026357458
    },
    {
      "intent": "None",
      "score": 0.00214573368
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00157622492
    },
    {
      "intent": "Utilities.Confirm",
      "score": 7.379545E-05
    }
  ],
  "entities": [
    {
      "entity": "jill jones",
      "type": "Employee",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "media team",
      "type": "builtin.keyPhrase",
      "startIndex": 25,
      "endIndex": 34
    },
    {
      "entity": "public portal",
      "type": "builtin.keyPhrase",
      "startIndex": 43,
      "endIndex": 55
    },
    {
      "entity": "jill jones",
      "type": "builtin.keyPhrase",
      "startIndex": 0,
      "endIndex": 9
    }
  ],
  "sentimentAnalysis": {
    "label": "positive",
    "score": 0.8694164
  }
}
```

sentimentAnalysis, 0,86 puanla pozitiftir. 

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Yaklaşım analizi etkin olan bu uygulama, doğal dil sorgusu amacı tanımladı ve genel yaklaşımı puan olarak içeren ayıklanmış verileri döndürdü. 

Sohbet botunuz artık konuşmadaki bir sonraki adımı belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve konuşmadaki yaklaşım verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [C# ile LUIS uç nokta API'sini çağırma](luis-get-started-cs-get-intent.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
