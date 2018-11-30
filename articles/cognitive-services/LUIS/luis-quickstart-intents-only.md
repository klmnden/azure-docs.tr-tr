---
title: 'Öğretici 1: Özel LUIS uygulamasındaki amaçları bulma'
titleSuffix: Azure Cognitive Services
description: Bir kullanıcının amacını tahmin eden özel bir uygulama oluşturun. E-posta adresleri veya tarihler gibi konuşma metinlerinden çeşitli veri öğeleri ayıklamadığından bu uygulama en basit LUIS uygulaması türüdür.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 30c9f572d77caacbeecf5f15d74fd8517e9fa883
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426868"
---
# <a name="tutorial-1-build-custom-app-to-determine-user-intentions"></a>Öğretici 1: Kullanıcı amaçlarını belirlemek için özel bir uygulama geliştirin

Bu öğreticide, konuşmaya (metne) göre bir kullanıcının amacını tahmin eden özel bir İnsan Kaynakları (İK) uygulaması oluşturacaksınız. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.

Uygulama amacı, doğal konuşma dili metninin amacını belirlemektir. Bunlar **Amaçlar** şeklinde kategorilere ayrılır. Bu uygulamanın birkaç amacı vardır. İlk amaç olan **`GetJobInformation`**, kullanıcının şirket içindeki işler hakkında bilgi istediğini belirtir. İkinci amaç olan **`None`**, kullanıcının bu uygulamanın _etki alanı_ (kapsamı) dışında kalan tüm konuşmaları için kullanılır. Daha sonra üçüncü amaç olan **`ApplyForJob`**, bir iş başvurusuyla ilgili konuşma için eklenir. Üçüncü amaç `GetJobInformation` amacından farklıdır çünkü birisi iş başvurusu yaptığında iş bilgileri zaten bilinmelidir. Bununla birlikte sözcük seçimine bağlı olarak hangi amaç olduğunu belirlemek zorluk çıkarabilir çünkü her ikisi de işle ilgilidir.

JSON yanıtı döndürdükten sonra LUIS’in istekle işi biter. LUIS kullanıcı konuşmalarını yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. 

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni bir uygulama oluşturma 
> * Amaç oluşturma
> * Örnek konuşmalar ekleme
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * Uç noktadan amaç alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

1. [https://www.luis.ai](https://www.luis.ai) URL’si ile LUIS portalında oturum açın. 

2. **Yeni uygulama oluştur**'u seçin.  

    [![](media/luis-quickstart-intents-only/app-list.png "Language Understanding (LUIS) Uygulamalarım sayfasının ekran görüntüsü")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Açılan iletişim kutusunda, `HumanResources` adını girin ve varsayılan kültürü **İngilizce** olarak tutun. Açıklamayı boş bırakın.

    ![Yeni LUIS uygulaması](./media/luis-quickstart-intents-only/create-app.png)

    Sonra uygulama **Amaçlar** sayfasını **Yok** amacıyla birlikte gösterir.

## <a name="getjobinformation-intent"></a>GetJobInformation amacı

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin. Yeni amaç adı olarak `GetJobInformation` girin. Kullanıcı, şirketteki açık pozisyonlar hakkında bilgi istediğinde bu amaç tahmin edilir.

    ![](media/luis-quickstart-intents-only/create-intent.png "Language Understanding (LUIS) Yeni amaç iletişim kutusunun ekran görüntüsü")

2. _Örnek konuşmalar_ sağlayarak, LUIS’i bu amaç için ne tür konuşmaların tahmin edilmesi gerektiği konusunda eğitiyorsunuz. Bu amaca kullanıcının sormasını beklediğiniz birkaç konuşma girin; örneğin:

    | Örnek konuşmalar|
    |--|
    |Any new jobs posted today? (Bugün yayımlanan yeni iş ilanı var mı?)|
    |What positions are available for Senior Engineers? (Kıdemli Mühendisler için uygun olan pozisyonlar hangileri?)|
    |Is there any work in databases? (Veritabanı alanında iş var mı?)|
    |Looking for a new situation with responsibilities in accounting (Muhasebe alanında yeni bir iş arayışım mevcut)|
    |Where is the job listings (İş ilanları nerede?)|
    |New jobs? (Yeni iş var mı?)|
    |Are there any new positions in the Seattle office? (Seattle ofisinde yeni pozisyon var mı?)|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "MyStore amacı için yeni konuşma girme işleminin ekran görüntüsü")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    


## <a name="none-intent"></a>Amaç yok 
İstemci uygulamasının, konuşmanın uygulamanın konu etki alanı dışında olup olmadığını bilmesi gerekir. LUIS, bir konuşma için **Yok** amacını döndürdüğünde istemci uygulamanız kullanıcının konuşmayı sonlandırmak isteyip istemediğini sorabilir. Kullanıcının konuşmayı sonlandırmak istememesi durumunda istemci uygulaması konuşmaya devam etme talimatları verebilir. 

Konu etki alanı dışındaki bu örnek konuşmalar **Yok** amacına gruplandırılır. Bu bölümü boş bırakmayın. 

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

2. **None** (Yok) amacını seçin. Kullanıcınızın girebileceği ancak İnsan Kaynakları uygulamanızla ilgili olmayan üç konuşma girin. Uygulama iş ilanlarınızla ilgiliyse **Yok** konuşmalarına örnek olarak şunlar verilebilir:

    | Örnek konuşmalar|
    |--|
    |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
    |Order a pizza for me (Bana bir pizza söyle)|
    |Penguins in the ocean (Okyanustaki penguenler)|


## <a name="train"></a>Eğitim 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent"></a>Amaç alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `I'm looking for a job with Natural Language Processing` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. Bu konuşma, örnek konuşmalarından hiçbiriyle aynı değil. İyi bir test olduğundan `GetJobInformation` amacını en yüksek puanlı amaç olarak döndürmelidir. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

    Sonuçlar uygulamadaki **tüm hedefleri** içerir, şu anda 2 amaç var. Bu uygulamanın şu anda bir varlığı olmadığından varlıklar dizisi boştur. 

    JSON sonucu, en yüksek puanlı amacı **`topScoringIntent`** özelliği olarak tanımlar. Tüm puanlar 1 ile 0 arasındadır ve 1'e yakın olan puanlar daha iyidir. 

## <a name="applyforjob-intent"></a>ApplyForJob amacı
LUIS web sitesine geri dönün ve kullanıcı konuşmasının bir iş başvurusuyla ilgili olup olmadığını belirlemek için yeni bir amaç oluşturun.

1. Uygulama derleme ekranına dönmek için sağ üstten **Build** (Derle) öğesini seçin.

2. Sol menüden **Intents** (Amaçlar) öğesini seçin.

3. **Create new intent** (Yeni amaç oluştur) öğesini seçin ve `ApplyForJob` adını girin. 

    ![LUIS Create new intent (Yeni amaç oluştur) iletişim kutusu](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Bu amaca kullanıcının sormasını beklediğiniz birkaç konuşma girin; örneğin:

    | Örnek konuşmalar|
    |--|
    |I want to apply for the new accounting job (Yeni muhasebe işine başvurmak istiyorum)|
    |Fill out application for Job 123456 (123456 numaralı iş için başvuru yap)|
    |Submit resume for engineering position (Mühendislik pozisyonu için özgeçmişimi gönder)|
    |Here is my c.v. for position 654234 (654234 numaralı pozisyon için özgeçmişimi gönderiyorum)|
    |Job 567890 and my paperwork (567890 numaralı iş için belgelerim)|

    [![](media/luis-quickstart-intents-only/utterance-applyforjob.png "ApplyForJob amacı için yeni konuşma girme işleminin ekran görüntüsü")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    LUIS, şu an için amacın doğru olduğundan emin olmadığından etiketlenen amaç kırmızıyla işaretlenmiştir. Uygulamayı eğittiğinizde LUIS, konuşmaların doğru amaca ait olduğunu anlar. 

## <a name="train-again"></a>Yeniden eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Yeniden yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-again"></a>Yeniden amaç alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Yeni tarayıcı penceresinde URL'nin sonuna `Can I submit my resume for job 235986` yazın. 

    ```JSON
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

    Sonuçlar, yeni amaç olan **ApplyForJob** ile birlikte mevcut amaçları da içerir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, İnsan Kaynakları (İK) uygulamasını oluşturdu, 2 amaç oluşturdu, her amaca örnek konuşmalar ekledi, Yok amacına örnek konuşmalar ekledi, eğitti, yayımladı, ve uç noktada test etti. Bunlar, LUIS modeli oluşturmanın temel adımlarıdır. 

> [!div class="nextstepaction"]
> [Bu uygulamaya önceden derlenmiş amaçlar ve varlıklar ekleme](luis-tutorial-prebuilt-intents-entities.md)
