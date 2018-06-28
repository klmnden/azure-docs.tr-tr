---
title: 'Öğretici: Veri ayıklamak için bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide makine öğrenmesi verilerini ayıklama amacıyla amaçları ve örnek bir varlığı kullanan basit bir LUIS uygulaması oluşturmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/29/2018
ms.author: v-geberr
ms.openlocfilehash: 1e8647e34da3d34946a4f6ac298017f6d4c99de6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265369"
---
# <a name="tutorial-create-app-that-uses-simple-entity"></a>Öğretici: Örnek varlık kullanan bir uygulama oluşturma
Bu öğreticide **Simple** (Basit) varlığını kullanarak konuşmadan makine öğrenmesi verilerini ayıklamayı gösteren bir uygulama oluşturacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Basit varlıkları anlama 
> * SendMessage varlığı ile iletişim alanı için yeni bir LUIS uygulaması oluşturma
> * _None_ amacını ve örnek konuşmaları ekleme
> * Konuşmadaki ileti içeriğini ayıklamak için basit varlık ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="purpose-of-the-app"></a>Uygulamanın amacı
Bu uygulama, bir konuşmadaki verileri nasıl çekeceğinizi göstermektedir. Bir sohbet botundan alınmış olan aşağıdaki konuşmaya bir göz atın:

```JSON
Send a message telling them to stop
```

Amaç ileti göndermektir. Konuşmanın önemli verileri, iletinin kendisidir: `telling them to stop`.  

## <a name="purpose-of-the-simple-entity"></a>Basit varlığın amacı
Basit varlığın amacı, LUIS uygulamasına iletinin ne olduğunu ve konuşmanın hangi bölümünde bulunabileceğini öğretmektir. Konuşmanın ileti olan bölümü, sözcük seçimine ve konuşma uzunluğuna göre konuşmadan konuşmaya değişiklik gösterebilen iletidir. LUIS uygulamasının tüm amaçlardaki konuşmaların herhangi birinde bulunabilecek iletilerin örneklerine ihtiyacı vardır.  

Bu basit uygulama için ileti, konuşmanın sonunda olacaktır. 

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
1. [LUIS][LUIS] web sitesinde oturum açın. LUIS uç noktalarının yayımlanmasını istediğiniz bölgede oturum açtığınızdan emin olun.

2. [LUIS][LUIS] web sitesinde **Create new app** (Yeni uygulama oluştur) öğesini seçin.  

    ![LUIS uygulamalarının listesi](./media/luis-quickstart-primary-and-secondary-data/app-list.png)

3. Açılan iletişim kutusuna `MyCommunicator` adını girin. 

    ![LUIS uygulamalarının listesi](./media/luis-quickstart-primary-and-secondary-data/create-new-app-dialog.png)

4. İşlem tamamlandıktan sonra uygulamalar **Intents** (Amaçlar) sayfasını ve **None** (Yok) amacını gösterir. 

    [![](media/luis-quickstart-primary-and-secondary-data/intents-list.png "LUIS Intents (Amaçlar) sayfası ve None (Yok) amacının ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/intents-list.png#lightbox)

## <a name="create-a-new-intent"></a>Yeni amaç oluşturma

1. **Intents** (Amaçlar) sayfasında **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png "Create new intent (Yeni amaç oluştur) düğmesi vurgulanmış LUIS uygulamasının ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png#lightbox)

2. Yeni amaç adı olarak `SendMessage` girin. Bu amacın kullanıcı ileti göndermek istediğinde seçilmesi gerekir.

    Bu amacı oluşturarak tanımlamak istediğiniz birincil bilgi kategorisini oluşturmuş olursunuz. Bu kategoriye bir ad verdiğinizde LUIS sorgu sonuçlarını kullanan başka uygulamalar da uygun bir yanıt bulmak veya ilgili eylemi gerçekleştirmek için bu kategorinin adını kullanabilir. LUIS bu soruları yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. 

    ![Amaç adı olarak SendMessage yazın](./media/luis-quickstart-primary-and-secondary-data/create-new-intent-popup-dialog.png)

3. `SendMessage` amacına kullanıcının sormasını beklediğiniz yedi konuşma girin; örneğin:

    | Örnek konuşmalar|
    |--|
    |Reply with I got your message, I will have the answer tomorrow (İletinizi aldım, yarın dönüş yapacağım yazarak yanıtla)|
    |Send message of When will you be home? (Eve ne zaman geliyorsun? iletisini gönder)|
    |Text that I am busy (Meşgul olduğumu yaz)|
    |Tell them that it needs to be done today (Bugün yapılması gerektiğini söyle)|
    |IM that I am driving and will respond later (Araç kullandığımı ve daha sonra yanıtlayacağımı yaz)|
    |Compose message to David that says When was that? (David'e O ne zamandı? yazan bir ileti gönder)|
    |say greg hello (Greg'e selam söyle)|

    [![](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png "LUIS uygulamasına girilmiş konuşmaların ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>None amacına konuşma ekleme

LUIS uygulamasının **None** (Yok) amacında şu an için bir konuşma mevcut değildir. **None** amacında uygulanın yanıtlamasını istemediğiniz konuşmaların bulunması gerekir. Bu bölümü boş bırakmayın. 
    
1. Sol panelden **Intents** (Amaçlar) öğesini seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png "\"Intents\" (Amaçlar) düğmesi vurgulanmış LUIS uygulamasının ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png#lightbox)

2. **None** (Yok) amacını seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png "None (Yok) amacının seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png#lightbox)

3. Kullanıcının girebileceği ancak uygulamanızla ilgili olmayan üç konuşma girin. **None** (Yok) konuşmalarına iyi örnekler:

    | Örnek konuşmalar|
    |--|
    |Cancel! (İptal!)|
    |Good bye (Güle güle)|
    |What is going on? (Neler oluyor?)|
    
    Sohbet botu gibi LUIS çağrı uygulamanızda LUIS, bir konuşma için **None** (Yok) amacını döndürdüğünde kullanıcının konuşmayı sonlandırmak isteyip istemediğini sorabilir. Kullanıcının konuşmayı sonlandırmak istememesi durumunda bot devam etme talimatları verebilir. 

    [![](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png "LUIS uygulamasına None (Yok) amacı için girilmiş konuşmaların ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png#lightbox)

## <a name="create-a-simple-entity-to-extract-message"></a>İleti ayıklamak için basit bir varlık oluşturma 
1. Sol menüden **Intents** (Amaçlar) öğesini seçin.

    ![Intents (Amaçlar) bağlantısını seçin](./media/luis-quickstart-primary-and-secondary-data/select-intents-from-none-intent.png)

2. Amaç listesinden `SendMessage` öğesini seçin.

    ![SendMessage amacını seçin](./media/luis-quickstart-primary-and-secondary-data/select-sendmessage-intent.png)

3. `Reply with I got your message, I will have the answer tomorrow` konuşması içinde ileti gövdesinin ilk sözcüğünü (`I`) ve ileti gövdesinin son sözcüğünü (`tomorrow`) seçin. İletinin tüm sözcükleri seçilir ve en üstünde metin kutusu olan bir açılan menü görüntülenir.

    [![](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png "Konuşmada ileti için seçilen sözcüklerin ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png#lightbox)

4. Metin kutusuna varlık adı olarak `Message` yazın.

    [![](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png "Kutuya varlık adının yazıldığı ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png#lightbox)

5. Açılan menüden **Create new entity** (Yeni varlık oluştur) öğesini seçin. Varlığın amacı, iletinin gövdesi olan metni çekmektir. Bu LUIS uygulamasında metin, konuşmanın sonundadır ancak konuşma ve ileti herhangi bir uzunlukta olabilir. 

    [![](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png "Konuşmadan yeni varlık oluşturma ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png#lightbox)

6. Açılır pencerede varsayılan varlık türü **Simple** (Basit), varlık adı ise `Message` olarak belirtilmiştir. Bu ayarları değiştirmeden **Done** (Bitti) öğesini seçin.

    ![Varlık türünü doğrulama](./media/luis-quickstart-primary-and-secondary-data/entity-type.png)

7. Varlık oluşturulduğuna ve bir konuşma etiketlendiğine göre artık diğer konuşmaları bu varlıkla etiketleyebilirsiniz. Bir konuşmayı ve ardından iletinin ilk ve son sözcüklerini seçin. Açılan menüde `Message` varlığını seçin. İleti, varlıkta etiketlenmiş duruma gelir. Devam ederek kalan konuşmalardaki tüm ileti tümceciklerini etiketleyin.

    [![](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png "Tüm konuşmalardaki iletilerin etiketlendiğini gösteren ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png#lightbox)

    Konuşmaların varsayılan görünümü, **Entities view** (Varlıklar görünümü) olarak belirlenmiştir. Konuşmaların üzerindeki **Entities view** (Varlıklar görünümü) denetimini seçin. **Tokens view** (Belirteçler görünümü) bölümünde konuşma metni görüntülenir. 

    [![](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png "Tokens view (Belirteçler görünümü) bölümünde konuşmaları gösteren ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar amaçlar ve varlıklar (model) üzerinde yapılan değişiklikleri bilemez. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Train (Eğitim) düğmesini seçin](./media/luis-quickstart-primary-and-secondary-data/train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı bildirimi](./media/luis-quickstart-primary-and-secondary-data/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    [![](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png "Publish to production slot (Üretim yuvasında yayımla) düğmesi vurgulanmış Yayımlama sayfasının ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png#lightbox)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama
**Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. 

[![](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png "Uç nokta vurgulanmış Publish (Yayımla) sayfası ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png#lightbox)

Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. Adres çubuğundaki URL'nin sonuna gidip `text I'm driving and will be 30 minutes late to the meeting` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `SendMessage` konuşmaları döndürmelidir.

```
{
  "query": "text I'm driving and will be 30 minutes late to the meeting",
  "topScoringIntent": {
    "intent": "SendMessage",
    "score": 0.987501
  },
  "intents": [
    {
      "intent": "SendMessage",
      "score": 0.987501
    },
    {
      "intent": "None",
      "score": 0.111048922
    }
  ],
  "entities": [
    {
      "entity": "i ' m driving and will be 30 minutes late to the meeting",
      "type": "Message",
      "startIndex": 5,
      "endIndex": 58,
      "score": 0.162995353
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Yalnızca iki amaca ve bir varlığa sahip olan bu uygulama, doğal dil sorgu varlığını tanımladı ve ileti verilerini döndürdü. 

JSON sonucu, 0,987501 puanla en yüksek puanlı amacın `SendMessage` olduğunu göstermektedir. Tüm puanlar 1 ile 0 arasındadır ve 1'e yakın olan puanlar daha iyidir. `None` amacının puanı 0,111048922 olarak belirtilmiştir ve sıfıra çok daha yakındır. 

İleti verilerinin türü `Message`, değeri ise `i ' m driving and will be 30 minutes late to the meeting` olmuştur. 

Sohbet botunuz artık `SendMessage` birincil eylemini ve bu eylemin parametrelerinden biri olan ileti metnini belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak iletiyi üçüncü taraf bir API üzerinden iletebilir. Bot veya çağrı uygulaması için başka programlama seçenekleri varsa LUIS bu görevleri gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hiyerarşik varlık eklemeyi öğrenin](luis-quickstart-intent-and-hier-entity.md)


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
