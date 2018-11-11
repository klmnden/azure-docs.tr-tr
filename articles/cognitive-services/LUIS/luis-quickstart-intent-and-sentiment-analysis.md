---
title: "Öğretici 9: LUIS'de pozitif, negatif ve nötr dahil yaklaşım analizi"
titleSuffix: Azure Cognitive Services
description: Bu öğreticide konuşmalardaki pozitif, negatif ve nötr yaklaşımları ayıklamayı gösteren bir uygulama oluşturacaksınız. Yaklaşım konuşmanın tamamından belirlenir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 95d1c4ffe76cf4c652f347014a838f1250c0ca15
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277486"
---
# <a name="tutorial-9--extract-sentiment-of-overall-utterance"></a>Öğretici 9: Genel konuşmanın yaklaşımını ayıklama
Bu öğreticide konuşmalardaki pozitif, negatif ve nötr yaklaşımları ayıklamayı gösteren bir uygulama oluşturacaksınız. Yaklaşım konuşmanın tamamından belirlenir.

Yaklaşım analizi, bir kullanıcının konuşmasının pozitif, negatif veya nötr olma durumunu belirleme yeteneğidir. 

Aşağıdaki konuşmalarda yaklaşım örnekleri gösterilmektedir:

|Yaklaşım|Puan|İfade|
|:--|:--|:--|
|pozitif|0,91 |John W. Smith did a great job on the presentation in Paris. (John W. Smith, Paris'teki sunumda harika bir iş çıkardı.)|
|pozitif|0,84 |jill-jones@mycompany.com did fabulous work on the Parker sales pitch. (Kullanıcı Parker satış sunumunda harikaydı.)|

Yaklaşım analizi, tüm konuşmalar için geçerli olan bir yayımlama ayarıdır. Yaklaşım analizi konuşmanın tamamını sağladığından konuşmada yaklaşım belirten kelimeleri bulup etiketlemeniz gerekmez. 

Yayımlama ayarı olduğu için amaçlar veya varlıklar sayfalarında görmezsiniz. [Etkileşimli test](luis-interactive-test.md#view-sentiment-results) bölmesinde veya uç nokta URL'sinde test yaparken görebilirsiniz. 

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma 
> * Yaklaşım analizini yayımlama ayarı olarak ekleyin
> * Eğitim
> * Yayımlama
> * Konuşmanın yaklaşımını uç noktasından alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma

Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-keyphrase-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `sentiment` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="employeefeedback-intent"></a>EmployeeFeedback'in amacı 
Şirket üyelerinin çalışanlar hakkındaki geri bildirimini yakalamak için yeni bir amaç ekleyin. 

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin.

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

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Uygulamayı yaklaşım analizi içerecek şekilde yapılandırma
1. Sağ üst gezinti bölmesinden **Yönet**'i seçin, sonra sol menüden **Yayımlama ayarları**'nı seçin.

2. Bu ayar için iki durumlu **Yaklaşım Analizi**'ni etkinleştirin. 

    ![](./media/luis-quickstart-intent-and-sentiment-analysis/turn-on-sentiment-analysis-as-publish-setting.png)

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-sentiment-of-utterance-from-endpoint"></a>Konuşmanın yaklaşımını uç noktasından alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Jill Jones work with the media team on the public portal was amazing` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `EmployeeFeedback` amacını yaklaşım analizi ayıklanmış şekilde döndürmelidir.
    
    ```JSON
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

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici, bir bütün olarak konuşmadan yaklaşım değerleri ayıklamak için bir yayımlama ayarı olarak yaklaşım analizi ekler.

> [!div class="nextstepaction"] 
> [İK uygulamasında uç nokta konuşmasını gözden geçirme](luis-tutorial-review-endpoint-utterances.md) 

