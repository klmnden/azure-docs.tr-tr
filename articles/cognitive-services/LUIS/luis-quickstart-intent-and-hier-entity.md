---
title: 'Öğretici: Konum verilerini almak için bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide veri ayıklama amacıyla amaçları ve bir hiyerarşik varlığı kullanan basit bir LUIS uygulaması oluşturmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 07/04/2018
ms.author: v-geberr
ms.openlocfilehash: babfc2f82e17f3745af1d940df89763170a002bd
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929595"
---
# <a name="tutorial-5-add-hierarchical-entity"></a>Öğretici: 5. Hiyerarşik varlık ekleme
Bu öğreticide, bağlama bağlı ilgili veri parçalarını nasıl bulacağınızı gösteren bir uygulama oluşturacaksınız. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Hiyerarşik varlıkları ve bağlamsal olarak öğrenilen alt öğeleri anlama 
> * LUIS uygulamasını İnsan Kaynakları (İK) alanında kullanma 
> * Çıkış ve varış alt öğelerine sahip konum hiyerarşik varlığını ekleme
> * Uygulamayı eğitme ve yayımlama
> * Hiyerarşik alt öğeleri içeren LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama 

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md#luis-website) hesabına ihtiyacınız olacak.

## <a name="before-you-begin"></a>Başlamadan önce
[Liste varlıkları](luis-quickstart-intent-and-list-entity.md) öğreticisinde oluşturulan İnsan Kaynakları uygulamasına sahip değilseniz JSON verilerini [içe aktararak](luis-how-to-start-new-app.md#import-new-app) [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulama oluşturun. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-list-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `hier` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="purpose-of-the-app-with-this-entity"></a>Bu varlığa sahip uygulamanın amacı
Bu uygulama bir çalışanın hangi kaynak konumdan (bina ve ofis) hangi hedef konuma (bina ve ofis) taşınacağını belirler. Konuşma içindeki konumları belirlemek için hiyerarşik varlığı kullanır. 

İki veri parçası şu özelliklere sahip olduğundan hiyerarşik varlık bu veri türü için iyi bir seçimdir:

* Konuşma bağlamında birbiriyle ilişkilidir.
* Her bir konumu belirtmek için belirli bir sözcük kullanır. Bu sözcüklere örnekler şunlardır: from/to, leaving/headed to, away from/toward (çıkış/varış, ayrılıyor/gidiyor, kaynaktan/hedefe doğru)
* Konumların ikisi de genelde aynı konuşmada bulunur. 

**Hiyerarşik** varlığın amacı, bağlama bağlı olarak konuşma içindeki ilgili verileri bulmaktır. Aşağıdaki konuşmaya bir göz atın:

```JSON
mv Jill Jones from a-2349 to b-1298
```
Konuşmada `a-2349` ve `b-1298` olmak üzere iki konum belirtilmiştir. Harfin bina adına, sayının da bu bina içindeki ofislere karşılık geldiğini düşünün. Veri parçalarının konuşmadan ayıklanması gerektiğinden ve birbirleriyle ilişkili olduğundan ikisinin de `Locations` hiyerarşik varlığının alt öğeleri olarak gruplanmış olması normaldir. 
 
Hiyerarşik varlığın alt öğelerinden yalnızca bir tanesi (çıkış veya varış) mevcut olduğunda da ayıklama işlemi gerçekleştirilir. Bir veya birden fazla öğenin ayıklanması için tüm alt öğelerin bulunması gerekmez. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Uygulamadaki önceden oluşturulmuş sayı varlığını kaldırma
Konuşmanın tamamını görmek ve hiyerarşik alt öğeleri işaretlemek için önceden oluşturulmuş sayı varlığını geçici olarak kaldırın.

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

    [ ![Sağ taraftaki menü çubuğunun en üstünde bulunan Build (Derleme) ifadesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-first-image.png)](./media/luis-quickstart-intent-and-hier-entity/hr-first-image.png#lightbox)

2. Sol menüden **Entities** (Varlıklar) öğesini seçin.

    [ ![Sol menüde Entities (Varlıklar) düğmesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-entities-button.png#lightbox)


3. Listedeki sayı varlığının sağ tarafında bulunan üç nokta (***...***) düğmesini seçin. **Sil**’i seçin. 

    [ ![Önceden oluşturulmuş Sayı varlığı için silme düğmesi vurgulanmış LUIS uygulaması varlık listesi sayfasının ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png)](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png#lightbox)


## <a name="add-utterances-to-moveemployee-intent"></a>MoveEmployee amacına konuşmalar ekleme

1. Sol menüden **Intents** (Amaçlar) öğesini seçin.

    [ ![Sol menüde Intents (Amaçlar) düğmesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-select-intents-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-intents-button.png#lightbox)

2. Amaç listesinden **MoveEmployee** girişini seçin.

    [ ![Sol menüde MoveEmployee amacı vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png)](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png#lightbox)

3. Aşağıdaki örnek konuşmaları ekleyin:

    |Örnek konuşmalar|
    |--|
    |Move John W. Smith **to** a-2345 (John W. Smith'i a-2345'e taşıyın)|
    |Direct Jill Jones **to** b-3499 (Jill Jones'u b-3499'a yönlendirin)|
    |Organize the move of x23456 **from** hh-2345 **to** e-0234 (x23456 numaralı ofisin hh-2345'ten e-0234'e taşınmasını organize edin)|
    |Begin paperwork to set x12345 **leaving** a-3459 **headed to** f-34567 (a-3459'dan f-34567'ye geçen x12345 için gerekli evrakları hazırlayın)|
    |Displace 425-555-0000 **away from** g-2323 **toward** hh-2345 (425-555-0000 numaralı hattı g-2323'ten hh-2345'e yönlendirin)|

    [Liste varlığı](luis-quickstart-intent-and-list-entity.md) öğreticisinde bir çalışan ad, e-posta adresi, dahili telefon, cep telefonu numarası veya ABD federal sosyal güvenlik numarası ile tanımlanabiliyordu. Bu çalışan numaraları konuşmalarda kullanılmaktadır. Yukarıdaki örnek konuşmalarda kaynak ve hedef konumlar kalın yazı tipiyle gösterilen farklı biçimlere sahiptir. Yalnızca birkaç konuşmada bilinçli olarak hedefler belirtilmiştir. Bu durum, LUIS uygulamasının kaynak belirtilmediğinde bu konumların konuşmadaki yerini anlamasına yardımcı olmaktadır.

    [ ![MoveEmployee amacındaki yeni konuşmaları gösteren LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)
     

## <a name="create-a-location-entity"></a>Konum varlığı oluşturma
LUIS uygulamasının konuşmalardaki kaynak ve hedef konumları etiketleyerek konumun ne olduğunu anlaması gerekir. Konuşmayı belirteç (ham) görünümünde görmek isterseniz konuşmaların üzerinde çubukta yer alan **Entities View** (Varlık Görünümü) denetimini seçin. Anahtarı açık duruma getirdikten sonra denetim **Tokens View** (Belirteç Görünümü) olarak etiketlenir.

1. `Displace 425-555-0000 away from g-2323 toward hh-2345` konuşmasının içinde `g-2323` sözcüğünü seçin. En üstte bir metin kutusu bulunan açılan menü görüntülenir. `Locations` metin kutusuna varlık adını girip açılan menüden **Create new entity** (Yeni varlık oluştur) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png "Amaç sayfasında yeni varlık oluşturma ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png#lightbox)

2. Açılır pencerede **Hierarchical** (Hiyerarşik) varlık türünü ve `Origin` ile `Destination` alt varlıkları seçin. **Done** (Bitti) öğesini seçin.

    ![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Yeni Location (Konum) varlığı için varlık oluşturma açılır iletişim kutusunun ekran görüntüsü")

3. LUIS, terimin çıkış mı, varış mı yoksa ikisi dışında bir değer mi olduğunu bilmediğinden `g-2323` etiketi `Locations` olarak işaretlenir. `g-2323` öğesini ve ardından **Locations** (Konumlar) girişini seçip sağ taraftaki menüden `Origin` öğesini seçin.

    [![](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png "Konum varlığı alt öğesini değiştirmek için varlık etiketleme açılır iletişim kutusunun ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png#lightbox)

5. Konuşmadaki binayı ve ofisi seçtikten sonra Locations (Konumlar) öğesini ve ardından sağdaki menüden `Origin` veya `Destination` öğesini seçerek diğer konuşmalardaki konumları etiketleyin. Tüm konumlar etiketlendikten sonra **Tokens View** (Belirteç Görünümü) desen gibi görünmeye başlar. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png "Konuşmalarda etiketlenmiş olan Locations (Konumlar) varlığının ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png#lightbox)

## <a name="add-prebuilt-number-entity-to-app"></a>Uygulamaya önceden oluşturulmuş sayı varlığı ekleme
Önceden oluşturulmuş sayı varlığını uygulamaya tekrar ekleyin.

1. Sol gezinti menüsünden **Entities** (Varlıklar) öğesini seçin.

    [ ![Sol gezinti bölmesinde vurgulanmış Entities (Varlıklar) düğmesinin ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-select-entity-button-from-intent-page.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-entity-button-from-intent-page.png#lightbox)

2. **Manage prebuilt entities** (Önceden oluşturulan varlıkları yönet) düğmesini seçin.

    [ ![Manage prebuilt entities (Önceden oluşturulan varlıkları yönet) düğmesi vurgulanmış Entities (Varlıklar) listesinin ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png#lightbox)

3. Önceden oluşturulmuş varlık listesinden **sayıyı** ve ardından **Done** (Bitti) öğesini seçin.

    ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar amaçlar ve varlıklar (model) üzerinde yapılan değişiklikleri bilemez. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Uygulamayı eğitme](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı oldu](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Publish to production slot (Üretim yuvasında yayımla) düğmesi vurgulanmış Yayımlama sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Uç nokta URL'si vurgulanmış Publish (Yayımla) sayfası ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

2. Adres çubuğundaki URL'nin sonuna gidip `Please relocation jill-jones@mycompany.com from x-2345 to g-23456` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `MoveEmployee` amacını hiyerarşik varlık ayıklanmış şekilde döndürmelidir.

```JSON
{
  "query": "Please relocation jill-jones@mycompany.com from x-2345 to g-23456",
  "topScoringIntent": {
    "intent": "MoveEmployee",
    "score": 0.9966052
  },
  "intents": [
    {
      "intent": "MoveEmployee",
      "score": 0.9966052
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.0325253047
    },
    {
      "intent": "FindForm",
      "score": 0.006137873
    },
    {
      "intent": "GetJobInformation",
      "score": 0.00462633232
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00415637763
    },
    {
      "intent": "ApplyForJob",
      "score": 0.00382325822
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00249120337
    },
    {
      "intent": "None",
      "score": 0.00130756292
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00119622645
    },
    {
      "intent": "Utilities.Confirm",
      "score": 1.26910036E-05
    }
  ],
  "entities": [
    {
      "entity": "jill - jones @ mycompany . com",
      "type": "Employee",
      "startIndex": 18,
      "endIndex": 41,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "x - 2345",
      "type": "Locations::Origin",
      "startIndex": 48,
      "endIndex": 53,
      "score": 0.8520272
    },
    {
      "entity": "g - 23456",
      "type": "Locations::Destination",
      "startIndex": 58,
      "endIndex": 64,
      "score": 0.974032
    },
    {
      "entity": "-2345",
      "type": "builtin.number",
      "startIndex": 49,
      "endIndex": 53,
      "resolution": {
        "value": "-2345"
      }
    },
    {
      "entity": "-23456",
      "type": "builtin.number",
      "startIndex": 59,
      "endIndex": 64,
      "resolution": {
        "value": "-23456"
      }
    }
  ]
}
```

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>Her konum için normal ifade kullanmak mümkün mü?
Evet, kaynak ve hedef rollerle bir normal ifade oluşturup desen içinde kullanabilirsiniz.

Bu örnekteki konumlarda `a-1234` gibi bir veya iki harf, kısa çizgi ve 4 ya da 5 basamaklı sayı kullanılan bir biçim kullanılmıştır. Bu veriler her bir konum için bir role sahip olan normal ifade varlığı olarak tanımlanabilir. Roller, desenlerle birlikte kullanılabilir. Bu konuşmaları temel alan desenler oluşturduktan sonra konum biçimi için bir normal ifade oluşturup desenlere ekleyebilirsiniz. <!-- Go to this tutorial to see how that is done -->

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Yalnızca birkaç amaca ve bir hiyerarşik varlığa sahip olan bu uygulama, doğal dil sorgu varlığını tanımladı ve ayıklanan verileri döndürdü. 

Sohbet botunuz artık `MoveEmployee` birincil eylemini ve konuşmada bulunan konum bilgilerini belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta (***...***) düğmesini ve sonra da **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Bileşik varlık eklemeyi öğrenin](luis-tutorial-composite-entity.md) 