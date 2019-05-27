---
title: Amaçları tahmin edin
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, bir kullanıcının engellemekse tahmin eden özel bir uygulama oluşturun. E-posta adresleri veya tarihler gibi konuşma metinlerinden çeşitli veri öğeleri ayıklamadığından bu uygulama en basit LUIS uygulaması türüdür.
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: 067829a1d9425ede1320242e364eca7c30bb7053
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66123918"
---
# <a name="tutorial-build-luis-app-to-determine-user-intentions"></a>Öğretici: Kullanıcı amaçları belirlemek için LUIS uygulaması oluşturma

Bu öğreticide, konuşmaya (metne) göre bir kullanıcının amacını tahmin eden özel bir İnsan Kaynakları (İK) uygulaması oluşturacaksınız. 

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni bir uygulama oluşturma 
> * Amaç oluşturma
> * Örnek konuşmalar ekleme
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * Uç noktadan amaç alma


[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="user-intentions-as-intents"></a>Hedefleri olarak kullanıcı amaçları

Uygulama amacı, konuşma, doğal dil metinlerinde amacınıza belirlemektir: 

`Are there any new positions in the Seattle office?`

Bunlar **Amaçlar** şeklinde kategorilere ayrılır. 

Bu uygulamanın birkaç amacı vardır. 

|Amaç|Amaç|
|--|--|
|ApplyForJob|Kullanıcı bir iş uyguluyor belirleyin.|
|GetJobInformation|Kullanıcı işleri genel olarak veya belirli bir işin hakkında bilgi için arıyor, belirleyin.|
|None|Kullanıcı bir şey istemesi durumunda belirlemek uygulama yanıtlamak için gereken değil. Bu amaç, uygulama oluşturmanın bir parçası sağlanan ve silinemez. |

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-intent-for-job-information"></a>İş bilgileri için hedefi oluşturma

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin. Yeni amaç adı olarak `GetJobInformation` girin. Bir kullanıcı şirket açık işler hakkında bilgi istediğinde bu hedefi tahmin edildiğinde. 

    ![Ekran görüntüsü, arama Language Understanding (LUIS) yeni hedefi iletişim](media/luis-quickstart-intents-only/create-intent.png "ekran görüntüsü, arama Language Understanding (LUIS) yeni hedefi iletişim kutusu")

1. **Done** (Bitti) öğesini seçin.

2. Birkaç örnek konuşma beklediğiniz istemek için bir kullanıcı bu amaç için ekleme:

    | Örnek konuşmalar|
    |--|
    |Any new jobs posted today? (Bugün yayımlanan yeni iş ilanı var mı?)|
    |Are there any new positions in the Seattle office? (Seattle ofisinde yeni pozisyon var mı?)|
    |Herhangi bir uzak çalışan vardır veya mühendislerinin işleri açık işletmenizi?|
    |Is there any work in databases? (Veritabanı alanında iş var mı?)|
    |Bir ortak çalışma durum tampa Office arıyorum.|
    |San francisco ofiste bir Stajyerlik mı?|
    |Yarı zamanlı işlerinizi üniversite kişilere var mı?|
    |Looking for a new situation with responsibilities in accounting (Muhasebe alanında yeni bir iş arayışım mevcut)|
    |New york city işinde için iki dilli konuşmacıları aranıyor.|
    |Hesap sorumlulukları ile yeni bir durum aranıyor.|
    |New jobs? (Yeni iş var mı?)|
    |Son 2 gün içinde eklenen mühendisleri için tüm işleri göster.|
    |Günümüzde iş gönderilerinin?|
    |Hangi hesap konumları Londra Office'te Aç misiniz?|
    |What positions are available for Senior Engineers? (Kıdemli Mühendisler için uygun olan pozisyonlar hangileri?)|
    |Where is the job listings (İş ilanları nerede?)|

    [![Yeni Konuşma DepolamaAlanım hedefi için girme ekran görüntüsü](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "DepolamaAlanım hedefi için yeni konuşma girme ekran görüntüsü")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    Sağlayarak _örnek konuşma_, ne tür bir konuşma bu amaç için tahmin edilen hakkında eğitim LUIS olan. 

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    

## <a name="add-example-utterances-to-the-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-before-testing-or-publishing"></a>Uygulamayı test etme ve yayımlama önce eğitin

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Uç noktasından sorguya uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-prediction-from-the-endpoint"></a>Uç noktasından hedefi öngörü Al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Adres çubuğundaki URL'nin sonuna gidip `I'm looking for a job with Natural Language Processing` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. Bu konuşma, örnek konuşmalarından hiçbiriyle aynı değil. İyi bir test olduğundan `GetJobInformation` amacını en yüksek puanlı amaç olarak döndürmelidir. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.9923871
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.9923871
        },
        {
          "intent": "None",
          "score": 0.007810574
        }
      ],
      "entities": []
    }
    ```

    `verbose=true` Querystring parametresi dahil anlamına gelir; **tüm hedefleri** uygulamanın sorgu sonuçları. Bu uygulamanın şu anda bir varlığı olmadığından varlıklar dizisi boştur. 

    JSON sonucu, en yüksek puanlı amacı **`topScoringIntent`** özelliği olarak tanımlar. Tüm puanlar 1 ile 0 arasındadır ve 1'e yakın olan puanlar daha iyidir. 

## <a name="create-intent-for-job-applications"></a>İş uygulamaları için hedefi oluşturma

LUIS portala geri dönün ve bir proje için uygulama hakkında kullanıcı utterance olup olmadığını belirlemek için yeni bir hedefi oluşturma.

1. Uygulama derleme ekranına dönmek için sağ üstten **Build** (Derle) öğesini seçin.

1. Seçin **hedefleri** ıntents listesini almak için sol menüden.

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin ve `ApplyForJob` adını girin. 

    ![LUIS Create new intent (Yeni amaç oluştur) iletişim kutusu](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

1. Bu amaca kullanıcının sormasını beklediğiniz birkaç konuşma girin; örneğin:

    | Örnek konuşmalar|
    |--|
    |Fill out application for Job 123456 (123456 numaralı iş için başvuru yap)|
    |Here is my c.v. for position 654234 (654234 numaralı pozisyon için özgeçmişimi gönderiyorum)|
    |My sürdürme zamanlı resepsiyonist gönderi için aşağıda verilmiştir.|
    |Bu belgeler resim Masası işlemiyle için uygulama.|
    |Araştırma ve geliştirme, San Diego Yaz üniversite Stajyerlik için uygulama|
    |My sürdürme kafeterya geçici konuma göndermek istiyorum.|
    |My sürdürme Kolomb, OH yeni Autocar ekibinden sorumlu gönderme|
    |I want to apply for the new accounting job (Yeni muhasebe işine başvurmak istiyorum)|
    |İş 456789 muhasebe Stajyerlik belgeleri aşağıda verilmiştir|
    |Job 567890 and my paperwork (567890 numaralı iş için belgelerim)|
    |My incelemeler tulsa muhasebe Stajyerlik için eklenir.|
    |Tatil teslim konumunun My belgeleri|
    |Lütfen seattle'my edecek yeni hesap oluşturma işi Gönder|
    |Submit resume for engineering position (Mühendislik pozisyonu için özgeçmişimi gönder)|
    |This is my c.v. POST 234123 Tampa içinde.|

<!--

    [![Screenshot of entering new utterances for ApplyForJob intent](media/luis-quickstart-intents-only/utterance-applyforjob.png "Screenshot of entering new utterances for ApplyForJob intent")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    The labeled intent is outlined in red because LUIS is currently uncertain the intent is correct. Training the app tells LUIS the utterances are on the correct intent. 

-->

## <a name="train-again"></a>Yeniden eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Yeniden yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-prediction-again"></a>Yeniden hedefi öngörü Al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Yeni tarayıcı penceresinde URL'nin sonuna `Can I submit my resume for job 235986` yazın. 

    ```json
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9634406
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9634406
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0171300638
        },
        {
          "intent": "None",
          "score": 0.00670867041
        }
      ],
      "entities": []
    }
    ```

    Sonuçlar, yeni amaç olan **ApplyForJob** ile birlikte mevcut amaçları da içerir. 

## <a name="client-application-next-steps"></a>İstemci uygulaması sonraki adımlar

JSON yanıtı döndürdükten sonra LUIS’in istekle işi biter. LUIS kullanıcı konuşmalarını yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. Konuşma izleme, Azure robot gibi istemci uygulaması tarafından sağlanır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık türleri](luis-concept-entity-types.md)
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUIS portalında test etme](luis-interactive-test.md)
* [Azure robot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, İnsan Kaynakları (İK) uygulamasını oluşturdu, 2 amaç oluşturdu, her amaca örnek konuşmalar ekledi, Yok amacına örnek konuşmalar ekledi, eğitti, yayımladı, ve uç noktada test etti. Bunlar, LUIS modeli oluşturmanın temel adımlarıdır. 

Bu uygulama ile devam [basit bir varlık ve deyim listesi ekleme](luis-quickstart-primary-and-secondary-data.md).

> [!div class="nextstepaction"]
> [Bu uygulamaya önceden derlenmiş amaçlar ve varlıklar ekleme](luis-tutorial-prebuilt-intents-entities.md)




