---
title: Varlığın, ifade listesi
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, basit varlığı kullanan bir utterance verilerin çalışan iş adı makine öğrendiniz ayıklayın. Ayıklama doğruluğunu artırmak için, basit varlığa özgü terimlerin tümcecik listesini ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: ea9a2df1f06ba6836ef88bc57dc3f95fd31e1ee9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66124213"
---
# <a name="tutorial-extract-names-with-simple-entity-and-a-phrase-list"></a>Öğretici: Varlığın ve deyim listesi ile adlarını Ayıkla

Bu öğreticide, **Basit** varlığını kullanarak bir ifadeden iş adının makine öğrenmesi verilerini ayıklayın. Ayıklama doğruluğunu artırmak için, basit varlığa özgü terimlerin tümcecik listesini ekleyin.

Basit varlık, sözcükler veya tümceciklerde bulunan tek bir veri kavramını algılar.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içeri aktarma
> * Basit bir varlık ekleme 
> * Sinyal sözcükleri artırmak üzere ifade listesi ekleme
> * Eğit 
> * Yayımlama 
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]


## <a name="simple-entity"></a>Basit varlık

Bu öğretici, iş adını ayıklamak için yeni bir basit varlık ekler. Bu LUIS uygulamasındaki basit varlığın amacı, LUIS uygulamasına iş adının ne olduğunu ve konuşmanın hangi bölümünde bulunabileceğini öğretmektir. İfadenin iş adı olan bölümü, sözcük seçimine ve ifade uzunluğuna göre ifadeden ifadeye değişiklik gösterebilen iletidir. LUIS için, iş adlarını kullanan tüm amaçlar genelinde iş adlarının örnekleri gerekir.  

Aşağıdaki durumlarda bu veri tipi için basit varlık idealdir:

* Veri, tek bir kavram olduğunda.
* Veri, normal ifade gibi düzün biçimlendirilmediğinde.
* Veri, önceden derlenmiş telefon numarası veya veri varlığı gibi genel olmadığında.
* Veri, liste varlığı gibi bilinen sözcükler listesiyle tam olarak eşleşmediğinde.
* Diğer veri öğelerini bileşik bir varlık veya bağlamsal rolleri gibi veri içermiyor.

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

## <a name="import-example-app"></a>Örnek uygulamayı içeri aktarma

1.  İndirip kaydedin [uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/build-app/intentonly.json) Intents Öğreticisi.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `simple` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="mark-entities-in-example-utterances-of-an-intent"></a>Örnek konuşma bir hedefinin varlıklarda işaretle

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. **Intents** (Amaçlar) sayfasında **ApplyForJob** amacını seçin. 

1. `I want to apply for the new accounting job` konuşmasında `accounting` öğesini seçin, açılır menünün en üst kısmına `Job` yazın ve ardından açılır menüden **Create new entity** (Yeni varlık oluştur) girişini seçin. 

    [![LUIS ekran görüntüsü ile 'ApplyForJob' hedefi ile vurgulanmış varlık adımları oluşturma](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "LUIS ekran görüntüsü ile 'ApplyForJob' hedefi ile vurgulanmış varlık adımları oluşturma")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

1. Açılır pencerede varlığın adını ve türünü doğrulayıp **Done** (Bitti) öğesini seçin.

    ![İş adı ve basit türü görünen basit varlık oluşturma açılır iletişim kutusu](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

1. İşle ilgili kelimelerinizle kalan konuşma işaretlemek **iş** bir sözcük veya tümcecik seçip seçerek varlık **iş** açılır menüden. 

    [![Vurgulanan iş Varlık etiketleme LUIS ekran](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "vurgulanan iş Varlık etiketleme LUIS ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)


## <a name="add-more-example-utterances-and-mark-entity"></a>Daha fazla örnek Konuşma ekleme ve varlık işaretleyin

Basit varlıklar, tahmin, bir yüksek güvenilirliğe sahip olmak için birçok örneği gerekir. 
 
1. Daha fazla konuşma ekleyin ve iş sözcüklerini veya tümceciklerini **Job** (İş) varlığı olarak etiketleyin. 

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
    |I would like to apply for the position in photography. (Fotoğrafçılık alanındaki pozisyon için başvuruda bulunmak istiyorum.)|photography (fotoğrafçılık)|

## <a name="mark-job-entity-in-other-intents"></a>Diğer ıntents iş varlığında işaretle

1. Sol menüden **Intents** (Amaçlar) öğesini seçin.

1. Amaç listesinden **GetJobInformation** girişini seçin. 

1. Örnek konuşma işlerinde etiket

    Bir hedefi başka bir amaç'den daha fazla örnek konuşma varsa, en yüksek tahmin edilen This is olma olasılığı daha anlaşılabilmelidir. 

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulama hedefi değişiklikleri test edilebilir şekilde eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktasından sorgulanabilir, bu nedenle, uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktasından amacı ve varlık öngörü Al 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Here is my c.v. for the engineering job` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `ApplyForJob` konuşmaları döndürmelidir.

    ```json
    {
      "query": "Here is my c.v. for the engineering job",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.98052007
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.98052007
        },
        {
          "intent": "GetJobInformation",
          "score": 0.03424581
        },
        {
          "intent": "None",
          "score": 0.0015820954
        }
      ],
      "entities": [
        {
          "entity": "engineering",
          "type": "Job",
          "startIndex": 24,
          "endIndex": 34,
          "score": 0.668959737
        }
      ]
    }
    ```
    
    LUIS, doğru amacı (**ApplyForJob**) buldu ve `engineering` değeriyle doğru **İş** varlığını ayıkladı.


## <a name="names-are-tricky"></a>Adlar kafa karıştırıcı olabilir
LUIS uygulaması yüksek güven derecesiyle doğru amacı buldu ve iş adını ayıkladı ancak adlar kafa karıştırıcı olabilir. `This is the lead welder paperwork` konuşmasını deneyin.  

Aşağıdaki JSON kodunda LUIS doğru amaç olan `ApplyForJob` yanıt vermektedir ancak `lead welder` iş adını ayıklamamıştır. 

```json
{
  "query": "This is the lead welder paperwork",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.860295951
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.860295951
    },
    {
      "intent": "GetJobInformation",
      "score": 0.07265678
    },
    {
      "intent": "None",
      "score": 0.00482481951
    }
  ],
  "entities": []
}
```

Ad herhangi bir şey olabileceğinden LUIS, sinyali güçlendirecek bir tümcecik listesi olması halinde varlıkları daha doğru bir şekilde tahmin edebilir.

## <a name="to-boost-signal-of-the-job-related-words-add-a-phrase-list-of-job-related-words"></a>Boost sinyale işle ilgili sözcük, tümcecik listesini bir kelimelerin işle ilgili Ekle

Açık [işleri tümcecik list.csv](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) Azure örnekleri GitHub deposundan. 1. 000'den iş sözcük ve tümcecikleri listesidir. Listede size anlamlı gelen iş sözcüklerini bulun. İstediğiniz sözcükler ve tümcecikler listede değilse ekleyin.

1. LUIS uygulamasının **Build** (Derleme) bölümünde **Improve app performance** (Uygulama performansını geliştir) kısmındaki **Phrase lists** (Tümcecik listeleri) girişini seçin.

1. **Create new phrase list** (Yeni tümcecik listesi oluştur) öğesini seçin. 

1. Yeni tümcecik listesine `JobNames` adını verin ve jobs-phrase-list.csv dosyasındaki listeyi kopyalayıp **Values** (Değerler) metin kutusuna yapıştırın. Enter'a basın. 

    [![Ekran görüntüsü yeni ifade listesi iletişim kutusu açılır oluşturma](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Oluştur ekran görüntüsü yeni ifade listesi iletişim kutusu açılır")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Daha fazla sözcük tümcecik listeye eklenen istiyorsanız belirleyin **Recommand** daha sonra yeni gözden **ilişkili değerler** ilgilendiren ekleyin. 

    Tutmaya dikkat **birbirinin yerine bu değerleri** iade, çünkü bu değerleri tüm işler için eş anlamlı sözcükler olarak değerlendirilmelidir. Değiştirilebilir ve noninterchangeable hakkında daha fazla bilgi [listesi kavramları ifade](luis-concept-feature.md#how-to-use-phrase-lists).

1. Tümcecik listesini etkinleştirmek için **Save** (Kaydet) öğesini seçin.

    [![Ekran görüntüsü oluşturma yeni ifade listesi iletişim kutusu açılır sözcük, tümcecik değerler listesinde](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Oluştur ekran görüntüsü yeni ifade listesi iletişim kutusu açılır sözcük, tümcecik değerler listesinde")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

1. Eğitme ve yeniden ifade listesini kullanmak için uygulamayı yayımlayın.

1. Uç noktayı aynı konuşmayla yeniden sorgulayın: `This is the lead welder paperwork.`

    JSON yanıtı ayıklanan varlığı içerir:

    ```json
      {
      "query": "This is the lead welder paperwork.",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.983076453
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.983076453
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0120766377
        },
        {
          "intent": "None",
          "score": 0.00248388131
        }
      ],
      "entities": [
        {
          "entity": "lead welder",
          "type": "Job",
          "startIndex": 12,
          "endIndex": 22,
          "score": 0.8373154
        }
      ]
    }
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlıkları öğretici olmadan hedefleri](luis-quickstart-intents-only.md)
* [Varlığın](luis-concept-entity-types.md) kavramsal bilgiler
* [İfade listesi](luis-concept-feature.md) kavramsal bilgiler
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUIS portalında test etme](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide İnsan Kaynakları uygulaması, ifadelerdeki iş adlarını bulmak için makine öğrenmesi basit varlığı kullanır. İş adları çok çeşitli sözcükler veya tümcecikler olabileceğinden, uygulamaya iş adı sözcüklerini artırmak için bir tümcecik listesi gerekti. 

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş keyphrase varlığı ekleme](luis-quickstart-intent-and-key-phrase.md)
