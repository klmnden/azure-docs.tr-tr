---
title: 'Öğretici 7: LUIS’te tümcecik listesi ile basit varlık'
titleSuffix: Azure Cognitive Services
description: Bir ifadeden makine öğrenmesi verilerini ayıklama
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: f3e931344d2d2294c03756d630c688df1e5da9a8
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425268"
---
# <a name="tutorial-7-extract-names-with-simple-entity-and-phrase-list"></a>Öğretici 7: Basit varlık ve tümcecik listesi ile adları ayıklama

Bu öğreticide, **Basit** varlığını kullanarak bir ifadeden iş adının makine öğrenmesi verilerini ayıklayın. Ayıklama doğruluğunu artırmak için, basit varlığa özgü terimlerin tümcecik listesini ekleyin.

Bu öğretici, iş adını ayıklamak için yeni bir basit varlık ekler. Bu LUIS uygulamasındaki basit varlığın amacı, LUIS uygulamasına iş adının ne olduğunu ve konuşmanın hangi bölümünde bulunabileceğini öğretmektir. İfadenin iş adı olan bölümü, sözcük seçimine ve ifade uzunluğuna göre ifadeden ifadeye değişiklik gösterebilen iletidir. LUIS için, iş adlarını kullanan tüm amaçlar genelinde iş adlarının örnekleri gerekir.  

Aşağıdaki durumlarda bu veri tipi için basit varlık idealdir:

* Veri, tek bir kavram olduğunda.
* Veri, normal ifade gibi düzün biçimlendirilmediğinde.
* Veri, önceden derlenmiş telefon numarası veya veri varlığı gibi genel olmadığında.
* Veri, liste varlığı gibi bilinen sözcükler listesiyle tam olarak eşleşmediğinde.
* Veri, bileşik varlık veya hiyerarşik varlık gibi başka veri öğeleri içermediğinde.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * Uygulamadan iş ayıklamak için basit bir varlık ekleme
> * İş sözcüklerinin sinyalini güçlendirmek için tümcecik listesi ekleme
> * Eğitim 
> * Yayımlama 
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma

Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-composite-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `simple` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="simple-entity"></a>Basit varlık
Basit varlık, sözcükler veya tümceciklerde bulunan tek bir veri kavramını algılar.

Bir sohbet botundan alınmış olan aşağıdaki ifadelere göz atın:

|İfade|Ayıklanabilir iş adı|
|:--|:--|
|I want to apply for the new accounting job. (Yeni muhasebe işine başvurmak istiyorum.)|accounting (muhasebe)|
|Submit my resume for the engineering position. (Özgeçmişimi mühendislik pozisyonu için değerlendirmek üzere gönderin.)|engineering (mühendislik)|
|Fill out application for job 123456 (123456 numaralı iş için başvuru yapın)|123456|

İş adı isim, fiil veya birden fazla kelimeden oluşan bir tümcecik olabileceğinden belirlemek zordur. Örneğin:

|İşler|
|--|
|engineer (mühendis)|
|software engineer (yazılım mühendisi)|
|senior software engineer (kıdemli yazılım mühendisi)|
|engineering team lead (mühendislik ekibi lideri) |
|air traffic controller (hava trafik kontrolörü)|
|motor vehicle operator (motorlu araç operatörü)|
|ambulance driver (ambulans şoförü)|
|tender (ihale sorumlusu)|
|extruder (ekstrüder)|
|millwright (değirmenci)|

Bu LUIS uygulamasında birden fazla amaçta iş adları bulunmaktadır. LUIS, bu sözcükleri tüm amaçların ifadelerinde etiketleyerek iş adının ne olduğu ve ifadelerin hangi bölümünde yer aldığı konusunda daha fazla bilgi edinir.

Varlıklar, örnek ifadelerde işaretlendikten sonra, basit varlığın sinyalini güçlendirmek için tümcecik listesi eklenmesi önemlidir. Tümcecik listesi, tam eşleşme olarak **kullanılmaz** ve beklediğiniz her olası değer olması gerekmez. 

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. **Intents** (Amaçlar) sayfasında **ApplyForJob** amacını seçin. 

3. `I want to apply for the new accounting job` konuşmasında `accounting` öğesini seçin, açılır menünün en üst kısmına `Job` yazın ve ardından açılır menüden **Create new entity** (Yeni varlık oluştur) girişini seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "'ApplyForJob' amacı seçilmiş ve varlık oluşturma adımları vurgulanmış LUIS ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

4. Açılır pencerede varlığın adını ve türünü doğrulayıp **Done** (Bitti) öğesini seçin.

    ![İş adı ve basit türü görünen basit varlık oluşturma açılır iletişim kutusu](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

5. `Submit resume for engineering position` konuşmasında `engineering` kelimesini İş varlığı olarak etiketleyin. `engineering` kelimesini seçin ve açılır menüden **Job** (İş) girişini seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "Etiketlenen iş varlığı vurgulanmış LUIS ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)

    Tüm konuşmalar etiketlendi ancak beş konuşma, LUIS'e işle ilgili sözcükler ve tümcecikler hakkında eğitim vermek için yeterli değildir. Sayı değeri içeren işler normal ifade varlığı kullandığından daha fazla örneğe ihtiyaçları yoktur. Sözcük veya tümcecik olan işler için en az 15 tane daha örnek gerekir. 

6. Daha fazla konuşma ekleyin ve iş sözcüklerini veya tümceciklerini **Job** (İş) varlığı olarak etiketleyin. Bir işe alma hizmeti için kullanılan iş türleri tüm alanlar için ortaktır. İşlerin belirli bir sektörle ilgili olmasını istiyorsanız iş sözcüklerinin bu durumu yansıtması gerekir. 

    |İfade|İş varlığı|
    |:--|:--|
    |I'm applying for the Program Manager desk in R&D (Ar-Ge bölümündeki Program Yöneticisi pozisyonu için başvuru yapıyorum)|Program Manager (Program Yöneticisi)|
    |Here is my line cook application. (Şef başvurumu gönderiyorum.)|şef|
    |My resume for camp counselor is attached. (Kamp Danışmanı özgeçmişim ektedir.)|camp counselor (kamp danışmanı)|
    |This is my c.v. for administrative assistant. (Yönetici asistanı özgeçmişim budur.)|administrative assistant (yönetici asistanı)|
    |I want to apply for the management job in sales. (Satış departmanındaki yönetim işi için başvuru yapmak istiyorum.)|management, sales (yönetim, satış)|
    |This is my resume for the new accounting position. (Yeni muhasebe pozisyonu için özgeçmişimi ilginize sunarım.)|accounting (muhasebe)|
    |My application for barback is included. (Barmenlik başvurum ektedir.)|barback (barmenlik)|
    |I'm submitting my application for roofer and framer. (Çatı ve doğrama işleri için başvurumu gönderiyorum.)|roofer, framer (çatı, doğrama)|
    |My c.v. for bus driver is here. (Otobüs şoförü başvurumu iletiyorum.)|bus driver (otobüs şoförü)|
    |I'm a registered nurse. (Sertifikalı bir hemşireyim.) Here is my resume. (Özgeçmişim burada.)|registered nurse (sertifikalı hemşire)|
    |I would like to submit my paperwork for the teaching position I saw in the paper. (Gazetede gördüğüm öğretmenlik pozisyonu için gerekli belgelerimi göndermek istiyorum.)|teaching (öğretmenlik)|
    |This is my c.v. for the stocker post in fruits and vegetables. (Meyve ve sebze stok görevlisi ilanı için özgeçmişimi iletiyorum.)|stocker (stok görevlisi)|
    |Apply for tile work. (Döşeme işleri için başvuruyorum.)|tile (döşeme)|
    |Attached resume for landscape architect. (Peyzaj mimarı ilanı için özgeçmişimi ekte gönderiyorum.)|landscape architect (peyzaj mimarı)|
    |My curriculum vitae for professor of biology is enclosed. (Biyoloji öğretmenliği için özgeçmişimi ekte bulabilirsiniz.)|professor of biology (biyoloji öğretmenliği)|
    |I would like to apply for the position in photography. (Fotoğrafçılık alanındaki pozisyon için başvuruda bulunmak istiyorum.)|photography (fotoğrafçılık)|git 

## <a name="label-entity-in-example-utterances"></a>Örnek ifadelerdeki varlığı etiketleme

Etiketleme veya _işaretleme_ ile varlık, varlığın örnek ifadelerde bulunduğu LUIS’i gösterir.

1. Sol menüden **Intents** (Amaçlar) öğesini seçin.

2. Amaç listesinden **GetJobInformation** girişini seçin. 

3. Örnek konuşmalardaki işleri etiketleyin:

    |İfade|İş varlığı|
    |:--|:--|
    |Is there any work in databases? (Veritabanı alanında iş var mı?)|veritabanları|
    |Looking for a new situation with responsibilities in accounting (Muhasebe alanında yeni bir iş arayışım mevcut)|accounting (muhasebe)|
    |What positions are available for senior engineers? (Kıdemli mühendisler için uygun olan pozisyonlar hangileri?)|senior engineers (kıdemli mühendisler)|

    Başka örnek konuşmalar da vardır ancak bunlarda işle ilgili sözcük mevcut değildir.

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Here is my c.v. for the programmer job` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `ApplyForJob` konuşmaları döndürmelidir.

    ```JSON
    {
      "query": "Here is my c.v. for the programmer job",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9826467
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9826467
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0218927357
        },
        {
          "intent": "MoveEmployee",
          "score": 0.007849265
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00349470088
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00348804821
        },
        {
          "intent": "None",
          "score": 0.00319909188
        },
        {
          "intent": "FindForm",
          "score": 0.00222647213
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00211193133
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00172086991
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00138010911
        }
      ],
      "entities": [
        {
          "entity": "programmer",
          "type": "Job",
          "startIndex": 24,
          "endIndex": 33,
          "score": 0.5230502
        }
      ]
    }
    ```
    
    LUIS, doğru amacı (**ApplyForJob**) buldu ve `programmer` değeriyle doğru **İş** varlığını ayıkladı.


## <a name="names-are-tricky"></a>Adlar kafa karıştırıcı olabilir
LUIS uygulaması yüksek güven derecesiyle doğru amacı buldu ve iş adını ayıkladı ancak adlar kafa karıştırıcı olabilir. `This is the lead welder paperwork` konuşmasını deneyin.  

Aşağıdaki JSON kodunda LUIS doğru amaç olan `ApplyForJob` yanıt vermektedir ancak `lead welder` iş adını ayıklamamıştır. 

```JSON
{
  "query": "This is the lead welder paperwork.",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.468558252
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.468558252
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0102701457
    },
    {
      "intent": "MoveEmployee",
      "score": 0.009442534
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00639619166
    },
    {
      "intent": "None",
      "score": 0.005859333
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.005087704
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00315379258
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00259344373
    },
    {
      "intent": "FindForm",
      "score": 0.00193389168
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.000420796918
    }
  ],
  "entities": []
}
```

Ad herhangi bir şey olabileceğinden LUIS, sinyali güçlendirecek bir tümcecik listesi olması halinde varlıkları daha doğru bir şekilde tahmin edebilir.

## <a name="to-boost-signal-add-phrase-list"></a>Sinyali güçlendirmek için tümcecik listesi ekleme

LUIS-Samples Github deposundaki [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) dosyasını açın. Bu listede binin üzerinde iş sözcüğü ve tümceciği vardır. Listede size anlamlı gelen iş sözcüklerini bulun. İstediğiniz sözcükler ve tümcecikler listede değilse ekleyin.

1. LUIS uygulamasının **Build** (Derleme) bölümünde **Improve app performance** (Uygulama performansını geliştir) kısmındaki **Phrase lists** (Tümcecik listeleri) girişini seçin.

2. **Create new phrase list** (Yeni tümcecik listesi oluştur) öğesini seçin. 

3. Yeni tümcecik listesine `Job` adını verin ve jobs-phrase-list.csv dosyasındaki listeyi kopyalayıp **Values** (Değerler) metin kutusuna yapıştırın. Enter'a basın. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Create new phrase list (Yeni tümcecik listesi oluştur) iletişim kutusu açılmış ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Tümcecik listesine daha fazla sözcük eklemek istiyorsanız, **Related Values** (İlgili Değerler) girişlerini gözden geçirin ve ilgili olanları ekleyin. 

4. Tümcecik listesini etkinleştirmek için **Save** (Kaydet) öğesini seçin.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Create new phrase list (Yeni tümcecik listesi oluştur) iletişim kutusu ve tümcecik listesindeki değerler kutusunda sözcükler bulunan ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. Tümcecik listesini kullanmak için uygulamayı tekrar [eğitin](#train) ve [yayımlayın](#publish).

6. Uç noktayı aynı konuşmayla yeniden sorgulayın: `This is the lead welder paperwork.`

    JSON yanıtı ayıklanan varlığı içerir:

    ```JSON
    {
        "query": "This is the lead welder paperwork.",
        "topScoringIntent": {
            "intent": "ApplyForJob",
            "score": 0.920025647
        },
        "intents": [
            {
            "intent": "ApplyForJob",
            "score": 0.920025647
            },
            {
            "intent": "GetJobInformation",
            "score": 0.003800706
            },
            {
            "intent": "Utilities.StartOver",
            "score": 0.00299335527
            },
            {
            "intent": "MoveEmployee",
            "score": 0.0027167045
            },
            {
            "intent": "None",
            "score": 0.00259556063
            },
            {
            "intent": "FindForm",
            "score": 0.00224019377
            },
            {
            "intent": "Utilities.Stop",
            "score": 0.00200693542
            },
            {
            "intent": "Utilities.Cancel",
            "score": 0.00195913855
            },
            {
            "intent": "Utilities.Help",
            "score": 0.00162656687
            },
            {
            "intent": "Utilities.Confirm",
            "score": 0.0002851904
            }
        ],
        "entities": [
            {
            "entity": "lead welder",
            "type": "Job",
            "startIndex": 12,
            "endIndex": 22,
            "score": 0.8295959
            }
        ]
    }
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide İnsan Kaynakları uygulaması, ifadelerdeki iş adlarını bulmak için makine öğrenmesi basit varlığı kullanır. İş adları çok çeşitli sözcükler veya tümcecikler olabileceğinden, uygulamaya iş adı sözcüklerini artırmak için bir tümcecik listesi gerekti. 

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş keyphrase varlığı ekleme](luis-quickstart-intent-and-key-phrase.md)
