---
title: İki amaca sahip basit bir uygulama oluşturma - Azure | Microsoft Docs
description: Bu hızlı başlangıçta iki amaca sahip olan ancak kullanıcı konuşmalarını tanımlamak için bir amaca sahip olmayan basit bir LUIS uygulaması oluşturmayı öğreneceksiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 3f23ade2b0256c72c344e2a619227a79e3c79a47
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160124"
---
# <a name="tutorial-1-build-app-with-custom-domain"></a>Öğretici: 1. Özel etki alanıyla bir uygulama derleme
Bu öğreticide kullanıcının uygulamaya gönderdiği konuşmaya (metin) göre _amacını_ belirlemek için **amaçların** nasıl kullanılacağını gösteren bir uygulama oluşturacaksınız. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.

Bu uygulama, konuşmalardan veri ayıklamadığından en basit LUIS uygulaması türüdür. Yalnızca konuşmadan kullanıcının amacını belirler.

<!-- green checkmark -->
> [!div class="checklist"]
> * İnsan Kaynakları (İK) etki alanına yönelik yeni bir uygulama oluşturma 
> * GetJobInformation amacını ekleme
> * GetJobInformation amacına örnek konuşmalar ekleme 
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama
> * ApplyForJob amacını ekleme
> * ApplyForJob amacına örnek konuşmalar ekleme 
> * Uç noktayı eğitme, yayımlama ve tekrar sorgulama 

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="purpose-of-the-app"></a>Uygulamanın amacı
Bu uygulamanın birkaç amacı vardır. İlk amaç olan **`GetJobInformation`**, kullanıcının şirket içindeki işler hakkında bilgi istediğini belirtir. İkinci amaç olan **`None`**, diğer tüm konuşma türlerini belirtir. Hızlı başlangıcın ilerleyen bölümlerinde üçüncü amaç olan `ApplyForJob` eklenecektir. 

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
1. [LUIS](luis-reference-regions.md#luis-website) web sitesinde oturum açın. LUIS uç noktalarının yayımlanmasını istediğiniz [bölgede](luis-reference-regions.md#publishing-regions) oturum açtığınızdan emin olun.

2. [LUIS](luis-reference-regions.md#luis-website) web sitesinde **Create new app** (Yeni uygulama oluştur) öğesini seçin.  

    [![](media/luis-quickstart-intents-only/app-list.png "My Apps (Uygulamalarım) sayfasının ekran görüntüsü")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Açılan iletişim kutusuna `HumanResources` adını girin. Bu uygulama, şirketinizin İnsan Kaynakları departmanıyla ilgili soruları kapsar. Bu departman şirketteki açık pozisyonlar gibi işe alımla ilgili durumları yönetir.

    ![Yeni LUIS uygulaması](./media/luis-quickstart-intents-only/create-app.png)

4. İşlem tamamlandıktan sonra uygulamalar **Intents** (Amaçlar) sayfasını ve **None** (Yok) amacını gösterir. 

## <a name="create-getjobinformation-intention"></a>GetJobInformation amacını oluşturma
1. **Create new intent** (Yeni amaç oluştur) öğesini seçin. Yeni amaç adı olarak `GetJobInformation` girin. Kullanıcı, şirketinizdeki açık pozisyonlar hakkında bilgi istediğinde bu amaca sahip olduğu tahmin edilir.

    ![](media/luis-quickstart-intents-only/create-intent.png "New intent (Yeni içerik) iletişim kutusunun ekran görüntüsü")

    Bu amacı oluşturarak tanımlamak istediğiniz bilgi kategorisini oluşturmuş olursunuz. Bu kategoriye bir ad verdiğinizde LUIS sorgu sonuçlarını kullanan başka uygulamalar da uygun bir yanıt bulmak için bu kategorinin adını kullanabilir. LUIS bu soruları yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. 

2. Amaca kullanıcının sormasını beklediğiniz yedi konuşma girin; örneğin:

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

3. LUIS uygulamasının **None** (Yok) amacında şu an için bir konuşma mevcut değildir. Uygulamanın yanıtlamadığı konuşmalara ihtiyacı vardır. Bu bölümü boş bırakmayın. Sol panelden **Intents** (Amaçlar) öğesini seçin. 

4. **None** (Yok) amacını seçin. Kullanıcının girebileceği ancak uygulamanızla ilgili olmayan üç konuşma girin. Uygulamanız iş ilanlarıyla ilgiliyse iyi **None** (Yok) konuşmalarına örnek olarak şunlar verilebilir:

    | Örnek konuşmalar|
    |--|
    |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
    |Order a pizza for me (Bana bir pizza söyle)|
    |Penguins in the ocean (Okyanustaki penguenler)|

    Sohbet botu gibi LUIS çağrı uygulamasında LUIS, bir konuşma için **None** (Yok) amacını döndürdüğünde kullanıcının konuşmayı sonlandırmak isteyip istemediğini sorabilir. Kullanıcının konuşmayı sonlandırmak istememesi durumunda sohbet botu devam etme talimatları verebilir. 

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Uygulamayı uç noktasına yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="query-endpoint-for-getjobinformation-intent"></a>Uç noktayı GetJobInformation amacı için sorgulama

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `I'm looking for a job with Natual Language Processing` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. Bu konuşma, 4. adımdaki örnek konuşmalarla aynı olmadığından test için iyidir ve en yüksek puanlı amaç olarak `GetJobInformation` amacını döndürmelidir. 

    ```
    {
      "query": "I'm looking for a job with Natual Language Processing",
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

## <a name="create-applyforjob-intention"></a>ApplyForJob amacını oluşturma
LUIS web sitesinin tarayıcı sekmesine dönün ve işe başvurmak için yeni bir amaç oluşturun.

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

    Yeniden [eğitin ve yayımlayın](#train-and-publish-the-app). 

## <a name="query-endpoint-for-applyforjob-intent"></a>Uç noktayı ApplyForJob amacı için sorgulama

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Yeni tarayıcı penceresinde URL'nin sonuna `Can I submit my resume for job 235986` yazın. 

    ```
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

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Yalnızca birkaç amaca sahip olan bu uygulama, aynı amaca sahip olan ancak farklı bir şekilde ifade edilen bir doğal dil sorgusunu tanımladı. 

En yüksek puanlı amaç, JSON sonucu ile gösterilmiştir. Tüm puanlar 1 ile 0 arasındadır ve 1'e yakın olan puanlar daha iyidir. `GetJobInformation` ve `None` amaçlarının puanı sıfıra çok daha yakındır. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu alarak soruyu yanıtlamak için gerekli bilgileri (LUIS içinde depolanmaz) bulabilir veya sohbeti sonlandırabilir. Bunlar bot veya çağrı uygulaması için programlama ile gerçekleştirilen seçeneklerdir. LUIS bu görevi gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu uygulamaya önceden derlenmiş amaçlar ve varlıklar ekleme](luis-tutorial-prebuilt-intents-entities.md)
