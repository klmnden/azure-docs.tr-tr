---
title: 'Öğretici: Konum verilerini almak için bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide veri ayıklama amacıyla amaçları ve bir hiyerarşik varlığı kullanan basit bir LUIS uygulaması oluşturmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: v-geberr
ms.openlocfilehash: 2547407126943161ba604fa2f5e80b9186cae57e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266507"
---
# <a name="tutorial-create-app-that-uses-hierarchical-entity"></a>Öğretici: Hiyerarşik varlık kullanan bir uygulama oluşturma
Bu öğreticide, bağlama bağlı ilgili veri parçalarını nasıl bulacağınızı gösteren bir uygulama oluşturacaksınız. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Hiyerarşik varlıkları ve bağlamsal olarak öğrenilen alt öğeleri anlama 
> * Bookflight amacını kullanarak seyahat alanı için yeni bir LUIS uygulaması oluşturma
> * _None_ amacını ve örnek konuşmaları ekleme
> * Çıkış ve varış alt öğelerine sahip konum hiyerarşik varlığını ekleme
> * Uygulamayı eğitme ve yayımlama
> * Hiyerarşik alt öğeleri içeren LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama 

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="purpose-of-the-app-with-this-entity"></a>Bu varlığa sahip uygulamanın amacı
Bu uygulama, bir kullanıcının uçak bileti almak isteyip istemediğini belirler. Kullanıcı metni içindeki konumları, çıkış şehrini ve varış şehrini belirlemek için hiyerarşik varlığı kullanır. 

İki veri parçası şu özelliklere sahip olduğundan hiyerarşik varlık bu veri türü için iyi bir seçimdir:

* İkisi de genellikle şehir veya havaalanı kodu olarak belirtilen konum verisidir.
* Genellikle çıkış ve varış şehrini belirleyebilmek için sözcüklerin etrafında benzersiz sözcük seçenekleri bulunur. Bu sözcüklere şunlar dahildir: to, headed toward, from, leaving (şehrine, doğru gidiyorum, şehrinden, ayrılıyorum).
* Konumların ikisi de genelde aynı konuşmada bulunur. 

**Hiyerarşik** varlığın amacı, bağlama bağlı olarak konuşma içindeki ilgili verileri bulmaktır. Aşağıdaki konuşmaya bir göz atın:

```JSON
1 ticket from Seattle to Cairo`
```

Konuşmada iki konum belirtilmiştir. Biri çıkış şehri olan Seattle, diğeri ise varış şehri olan Kahire. Uçak bileti almak için bu iki şehir de önemlidir. Basit varlıklar kullanılarak bulunmalarının yanı sıra birbiriyle ilgilidir ve genelde aynı konuşmada yer alacaktır. Bu nedenle ikisinin de **"Konum"** hiyerarşik varlığının alt öğeleri olarak gruplanması mantıklıdır. 

Uygulamanın makine öğrenmesi varlıkları olarak çıkış ve varış şehirleri etiketli örnek konuşmalara ihtiyacı vardır. Bu LUIS uygulamasına varlıkların, konuşmaların hangi bölümünde olduğunu, hangi uzunlukta olduğunu ve etrafındaki kelimeleri öğretir. 

## <a name="app-intents"></a>Uygulama amaçları
Amaçlar, kullanıcı isteklerinin kategorileridir. Bu uygulamada iki amaç vardır: BookFlight ve None. [None](luis-concept-intent.md#none-intent-is-fallback-for-app) amacı, uygulama haricindeki bir durumu belirtmek için kullanılır.  

## <a name="hierarchical-entity-is-contextually-learned"></a>Hiyerarşik varlık, bağlamsal olarak öğrenilir 
Varlığın amacı, konuşma içindeki metnin bölümlerini bulmak ve kategorilere ayırmaktır. [Hiyerarşik](luis-concept-entity-types.md) varlık, kullanım bağlamına bağlı bir üst öğe-alt öğe varlığıdır. Bir kişi, `to` ve `from` kullanımına göre bir konuşma içindeki çıkış ve varış şehirlerini belirleyebilir. Bu, bağlamsal kullanıma bir örnektir.  

Bu Seyahat uygulaması için LUIS çıkış ve varış konumlarını standart rezervasyon oluşturacak ve ilgili alanları dolduracak şekilde ayıklar. LUIS konuşmalarda farklı sözcükler, kısaltmalar ve argo kullanılmasına izin verir. 

Kullanıcıların konuşmalarına basit örnekler şunlardır:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Researve a seat from New York to Paris on the first of April
```

Konuşmaların kısaltılmış veya argo sürümleri şunlardır:

```
LHR tomorrow
SEA to NYC next Monday
LA to MCO spring break
```
 
Hiyerarşik varlık, çıkış ve varış konumuyla eşleşir. Hiyerarşik varlığın alt öğelerinden yalnızca bir tanesi (çıkış veya varış) mevcut olduğunda da ayıklama işlemi gerçekleştirilir. Bir veya birden fazla öğenin ayıklanması için tüm alt öğelerin bulunması gerekmez. 

## <a name="what-luis-does"></a>LUIS tarafından gerçekleştirilen işlemler
Konuşmadaki amaç ve varlıklar tanımlandıktan, [ayıklandıktan](luis-concept-data-extraction.md#list-entity-data) ve [uç noktadan](https://aka.ms/luis-endpoint-apis) JSON biçiminde döndürüldükten sonra LUIS görevini tamamlamış olur. Arama uygulaması veya sohbet botu bu JSON yanıtını alıp tasarlanma amacına uygun olarak isteği gerçekleştirir. 

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
1. [LUIS][LUIS] web sitesinde oturum açın. LUIS uç noktalarının yayımlanmasını istediğiniz [bölgede][LUIS-regions] oturum açtığınızdan emin olun.

2. [LUIS][LUIS] web sitesinde **Create new app** (Yeni uygulama oluştur) öğesini seçin.  

    [![](media/luis-quickstart-intent-and-hier-entity/app-list.png "Uygulama listesi sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/app-list.png#lightbox)

3. Açılan iletişim kutusuna `MyTravelApp` adını girin. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-app.png "Create new app (Yeni uygulama oluştur) açılan iletişim kutusunun ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/create-new-app.png#lightbox)

4. İşlem tamamlandıktan sonra uygulamalar **Intents** (Amaçlar) sayfasını ve **None** (Yok) amacını gösterir. 

    [![](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png "Yalnızca None (Yok) amacına sahip Intents (Amaçlar) listesinin ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Yeni amaç oluşturma

1. **Intents** (Amaçlar) sayfasında **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png "Create new intent (Yeni amaç oluştur) düğmesi vurgulanmış Intents (Amaçlar) listesinin ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png#lightbox)

2. Yeni amaç adı olarak `BookFlight` girin. Bu amacın kullanıcı uçak bileti almak istediğinde seçilmesi gerekir.

    Bu amacı oluşturarak tanımlamak istediğiniz birincil bilgi kategorisini oluşturmuş olursunuz. Bu kategoriye bir ad verdiğinizde LUIS sorgu sonuçlarını kullanan başka uygulamalar da uygun bir yanıt bulmak veya ilgili eylemi gerçekleştirmek için bu kategorinin adını kullanabilir. LUIS bu soruları yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png "Create new intent (Yeni amaç oluştur) açılan iletişim kutusunun ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png#lightbox)

3. `BookFlight` amacına kullanıcının sormasını beklediğiniz birkaç konuşma girin; örneğin:

    | Örnek konuşmalar|
    |--|
    |Book 2 flights from Seattle to Cairo next Monday (Önümüzdeki Pazartesi günü Seattle'dan Kahire'ye 2 uçak bileti al)|
    |Reserve a ticket to London tomorrow (Yarınki Londra uçuşuna bir bilet al)|
    |Schedule 4 seats from Paris to London for April 1 (1 Nisan'daki Paris-Londra uçuşuna 4 kişilik bilet al)|

    [![](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png "BookFlight amaç sayfasında konuşma girme bölümünün gösterildiği ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>None amacına konuşma ekleme

LUIS uygulamasının **None** (Yok) amacında şu an için bir konuşma mevcut değildir. **None** amacında uygulanın yanıtlamasını istemediğiniz konuşmaların bulunması gerekir. Bu bölümü boş bırakmayın. 

1. Sol panelden **Intents** (Amaçlar) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png "Intents (Amaçlar) düğmesinin vurgulanmış olduğu BookFlight amaç sayfası")](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png#lightbox)

2. **None** (Yok) amacını seçin. Kullanıcının girebileceği ancak uygulamanızla ilgili olmayan üç konuşma girin:

    | Örnek konuşmalar|
    |--|
    |Cancel! (İptal!)|
    |Good bye (Güle güle)|
    |What is going on? (Neler oluyor?)|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Konuşmanın None (Yok) amacına uygun olduğu tahmin edildiğinde
LUIS çağrı uygulamanızda (sohbet botu gibi) LUIS, bir konuşma için **None** (Yok) amacını döndürdüğünde kullanıcının konuşmayı sonlandırmak isteyip istemediğini sorabilir. Kullanıcının konuşmayı sonlandırmak istememesi durumunda bot devam etme talimatları verebilir. 

Varlıklar **None** (Yok) amacı içinde çalışır. En yüksek puanlı amacın **None** (Yok) olması ancak sohbet botunuz için anlamlı olan bir varlık ayıklanması halinde sohbet botunuz müşterinin amacına odaklanan bir soru yöneltebilir. 

## <a name="create-a-location-entity-from-the-intent-page"></a>Intent (Amaç) sayfasından bir konum varlığı oluşturma
İki amaçta da konuşma bulunduğuna göre artık LUIS uygulamasının konumun ne olduğunu anlaması gerekiyor. `BookFlight` amacına dönün ve aşağıdaki adımları izleyerek konuşma içindeki şehir adını etiketleyin (işaretleyin):

1. Sol panelde **Intents** (Amaçlar) öğesini seçerek `BookFlight` amacına dönün.

2. Amaç listesinden `BookFlight` öğesini seçin.

3. `Book 2 flights from Seattle to Cairo next Monday` konuşmasının içinde `Seattle` sözcüğünü seçin. En üstünde yeni varlık oluşturmak kullanabileceğiniz bir metin kutusu bulunan açılan menü görüntülenir. `Location` metin kutusuna varlık adını girip açılan menüden **Create new entity** (Yeni varlık oluştur) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png "BookFlight amaç sayfası, seçilen metinden yeni varlık oluşturma adımının ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png#lightbox)

4. Açılır pencerede **Hierarchical** (Hiyerarşik) varlık türünü ve `Origin` ile `Destination` alt varlıkları seçin. **Done** (Bitti) öğesini seçin.

    [![](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png "Yeni Location (Konum) varlığı için varlık oluşturma açılır iletişim kutusunun ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png#lightbox)

    LUIS, terimin çıkış mı, varış mı yoksa ikisi dışında bir değer mi olduğunu bilmediğinden `Seattle` etiketi `Location` olarak işaretlenir. `Seattle` öğesini ve ardından Location (Konum) girişini seçip sağ taraftaki menüden `Origin` öğesini seçin.

5. Varlık oluşturuldu ve bir konuşma etiketlendi. Şimdi şehir adını, ardından Location (Konum) girişini, ardından da sağ taraftaki menüden `Origin` veya `Destination` öğesini seçerek diğer şehirleri etiketleyin.

    [![](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png "Varlık seçimi için konuşma metni seçilmiş Bookflight varlığının ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar amaçlar ve varlıklar (model) üzerinde yapılan değişiklikleri bilemez. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Uygulamayı eğitme](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı oldu](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish.png "Publish (Yayımla) düğmesi vurgulanmış Bookflight amacının ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/publish.png#lightbox)

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Publish to production slot (Üretim yuvasında yayımla) düğmesi vurgulanmış Yayımlama sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Uç nokta URL'si vurgulanmış Publish (Yayımla) sayfası ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

2. Adres çubuğundaki URL'nin sonuna gidip `1 ticket to Portland on Friday` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `BookFlight` amacını hiyerarşik varlık ayıklanmış şekilde döndürmelidir.

```
{
  "query": "1 ticket to Portland on Friday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9998226
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.9998226
    },
    {
      "intent": "None",
      "score": 0.221926212
    }
  ],
  "entities": [
    {
      "entity": "portland",
      "type": "Location::Destination",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.564448953
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Yalnızca iki amaca ve bir hiyerarşik varlığa sahip olan bu uygulama, doğal dil sorgu varlığını tanımladı ve ayıklanan verileri döndürdü. 

Sohbet botunuz artık `BookFlight` birincil eylemini ve konuşmada bulunan konum bilgilerini belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Liste varlığı eklemeyi öğrenin](luis-quickstart-intent-and-list-entity.md) 

Sayıları ayıklamak için **number** [önceden oluşturulmuş varlığını](luis-how-to-add-entities.md#add-prebuilt-entity) ekleyin. 

Tarih bilgilerini ayıklamak için **datetimeV2** [önceden oluşturulmuş varlığını](luis-how-to-add-entities.md#add-prebuilt-entity) ekleyin.


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
