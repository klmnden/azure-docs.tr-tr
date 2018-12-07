---
title: 'Öğretici 5: Üst Öğe/Alt Öğe ilişkileri - Bağlamsal olarak öğrenilen veriler için LUIS Hiyerarşik varlığı'
titleSuffix: Azure Cognitive Services
description: Bağlama göre ilgili veri parçaları bulabilirsiniz. Örneğin, bir bina ya da ofisten başka bir bina ya da ofise fiziksel olarak taşınmada çıkış ve varış konumları.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/05/2018
ms.author: diberry
ms.openlocfilehash: 36751f533b59e0ff140f5ad03e7f1fc0fa9cad41
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53000575"
---
# <a name="tutorial-5-extract-contextually-related-data"></a>Öğretici 5: Bağlamsal olarak ilişkili verileri ayıklama
Bu öğreticide bağlama göre ilgili veri parçalarını bulacaksınız. Örneğin, bir bina ya da ofisten başka bir bina ya da ofise fiziksel olarak taşınmada çıkış ve varış konumları. Bir çalışma sırası oluşturmak için her iki veri parçasının da mevcut ve birbirleriyle ilişkili olması gerekir.  

Bu uygulama bir çalışanın hangi kaynak konumdan (bina ve ofis) hangi hedef konuma (bina ve ofis) taşınacağını belirler. Konuşma içindeki konumları belirlemek için hiyerarşik varlığı kullanır. **Hiyerarşik** varlığın amacı, bağlama bağlı olarak konuşma içindeki ilgili verileri bulmaktır. 

İki veri parçası şu özelliklere sahip olduğundan hiyerarşik varlık bu veri türü için iyi bir seçimdir:

* Basit varlıklardır.
* Konuşma bağlamında birbiriyle ilişkilidir.
* Her bir konumu belirtmek için belirli bir sözcük kullanır. Bu sözcüklere örnekler şunlardır: from/to, leaving/headed to, away from/toward (çıkış/varış, ayrılıyor/gidiyor, kaynaktan/hedefe doğru)
* Konumların ikisi de genelde aynı konuşmada bulunur. 
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * Amaç ekleme 
> * Çıkış ve varış alt öğelerine sahip konum hiyerarşik varlığını ekleme
> * Eğitim
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma
Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-list-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `hier` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Uygulamadaki önceden oluşturulmuş sayı varlığını kaldırma
Tüm utterance görebilir ve hiyerarşik alt işaretlemek için [geçici olarak önceden oluşturulmuş sayı varlık kaldırma](luis-prebuilt-entities.md#marking-entities-containing-a-prebuilt-entity-token). 

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Sol menüden **Entities** (Varlıklar) öğesini seçin.

3. Listedeki sayı varlığının sol tarafındaki onay kutusunu seçin. **Sil**’i seçin. 

## <a name="add-utterances-to-moveemployee-intent"></a>MoveEmployee amacına konuşmalar ekleme

1. Sol menüden **Intents** (Amaçlar) öğesini seçin.

2. Amaç listesinden **MoveEmployee** girişini seçin.

3. Aşağıdaki örnek konuşmaları ekleyin:

    |Örnek konuşmalar|
    |--|
    |Move John W. Smith **to** a-2345 (John W. Smith'i a-2345'e taşıyın)|
    |Direct Jill Jones **to** b-3499 (Jill Jones'u b-3499'a yönlendirin)|
    |Organize the move of x23456 **from** hh-2345 **to** e-0234 (x23456 numaralı ofisin hh-2345'ten e-0234'e taşınmasını organize edin)|
    |Begin paperwork to set x12345 **leaving** a-3459 **headed to** f-34567 (a-3459'dan f-34567'ye geçen x12345 için gerekli evrakları hazırlayın)|
    |Displace 425-555-0000 **away from** g-2323 **toward** hh-2345 (425-555-0000 numaralı hattı g-2323'ten hh-2345'e yönlendirin)|

    [ ![MoveEmployee amacındaki yeni konuşmaları gösteren LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

    [Liste varlığı](luis-quickstart-intent-and-list-entity.md) öğreticisinde bir çalışan ad, e-posta adresi, dahili telefon, cep telefonu numarası veya ABD federal sosyal güvenlik numarası ile tanımlanmaktadır. Bu çalışan numaraları konuşmalarda kullanılmaktadır. Yukarıdaki örnek konuşmalarda kaynak ve hedef konumlar kalın yazı tipiyle gösterilen farklı biçimlere sahiptir. Yalnızca birkaç konuşmada bilinçli olarak hedefler belirtilmiştir. Bu durum, LUIS uygulamasının kaynak belirtilmediğinde bu konumların konuşmadaki yerini anlamasına yardımcı olmaktadır.     

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="create-a-location-entity"></a>Konum varlığı oluşturma
LUIS uygulamasının konuşmalardaki kaynak ve hedef konumları etiketleyerek konumun ne olduğunu anlaması gerekir. Konuşmayı belirteç (ham) görünümünde görmek isterseniz konuşmaların üzerinde çubukta yer alan **Entities View** (Varlık Görünümü) denetimini seçin. Anahtarı açık duruma getirdikten sonra denetim **Tokens View** (Belirteç Görünümü) olarak etiketlenir.

Aşağıdaki konuşmaya bir göz atın:

```JSON
mv Jill Jones from a-2349 to b-1298
```

Konuşmada `a-2349` ve `b-1298` olmak üzere iki konum belirtilmiştir. Harfin bina adına, sayının da bu bina içindeki ofislere karşılık geldiğini düşünün. İstemci uygulamadaki isteğin tamamlanması için konuşmadan her iki veri parçasının da ayıklanması gerektiği ve bunlar birbirleriyle ilişkili olduğu için ikisinin de `Locations` hiyerarşik varlığının alt öğeleri olarak gruplanması uygun olacaktır. 
 
Hiyerarşik varlığın alt öğelerinden yalnızca bir tanesi (çıkış veya varış) mevcut olduğunda da ayıklama işlemi gerçekleştirilir. Bir veya birden fazla öğenin ayıklanması için tüm alt öğelerin bulunması gerekmez. 

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

2. **Add prebuilt entity** (Önceden oluşturulan varlık ekle) düğmesini seçin.

3. Önceden oluşturulmuş varlık listesinden **sayıyı** ve ardından **Done** (Bitti) öğesini seçin.

    ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


2. Adres çubuğundaki URL'nin sonuna gidip `Please relocation jill-jones@mycompany.com from x-2345 to g-23456` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `MoveEmployee` amacını hiyerarşik varlık ayıklanmış şekilde döndürmelidir.

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
    
    Doğru amaç tahmin edilir ve varlıklar dizisinde ilgili **varlık** özelliğinde çıkış ve varış değerlerinin ikisi de vardır.
    

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>Her konum için normal ifade kullanmak mümkün mü?
Evet. Normal ifadeyi çıkış ve varış rolleriyle oluşturup bir desende kullananın.

Bu örnekteki konumlarda `a-1234` gibi bir veya iki harf, kısa çizgi ve 4 ya da 5 basamaklı sayı kullanılan bir biçim kullanılmıştır. Bu veriler her bir konum için bir role sahip olan normal ifade varlığı olarak tanımlanabilir. Roller yalnızca desenler için kullanılabilir. Bu konuşmaları temel alan desenler oluşturduktan sonra konum biçimi için bir normal ifade oluşturup desenlere ekleyebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="hierarchical-entities-versus-roles"></a>Hiyerarşik varlıklarla rollerin karşılaştırılması

Daha fazla bilgi için bkz. [Rollerle hiyerarşik varlıkların karşılaştırması](luis-concept-roles.md#roles-versus-hierarchical-entities).

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yeni bir amaç oluşturuldu ve çıkış ve varış konumlarının bağlamsal olarak öğrenilen verileri için örnek konuşmalar eklendi. Uygulama eğitilip yayımlandıktan sonra bir istemci uygulama bu bilgileri ilgili bilgilerle bir taşınma bileti oluşturmak için kullanabilir.

> [!div class="nextstepaction"] 
> [Bileşik varlık eklemeyi öğrenin](luis-tutorial-composite-entity.md) 
