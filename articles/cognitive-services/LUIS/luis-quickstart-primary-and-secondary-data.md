---
title: 'Öğretici: Veri ayıklamak için bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide makine öğrenmesi verilerini ayıklama amacıyla amaçları ve örnek bir varlığı kullanan basit bir LUIS uygulaması oluşturmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: v-geberr
ms.openlocfilehash: e6ab9d1db0144ffa68fe9dc3381ba31d57aa0cae
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130903"
---
# <a name="tutorial-6-add-simple-entity-and-phrase-list"></a>Öğretici: 6. Basit varlık ve tümcecik listesi ekleme
Bu öğreticide **Simple** (Basit) varlığını kullanarak konuşmadan makine öğrenmesi verilerini ayıklamayı gösteren bir uygulama oluşturacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Basit varlıkları anlama 
> * İnsan Kaynakları (İK) alanında yeni bir LUIS uygulaması oluşturma 
> * Uygulamadan iş ayıklamak için basit bir varlık ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama
> * İş sözcüklerinin sinyalini güçlendirmek için tümcecik listesi ekleme
> * Uygulamayı eğitip yayımlama ve uç noktayı yeniden sorgulama

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md#luis-website) hesabına ihtiyacınız olacak.

## <a name="before-you-begin"></a>Başlamadan önce
[Hiyerarşik varlık](luis-quickstart-intent-and-hier-entity.md) öğreticisinde oluşturulan İnsan Kaynakları uygulamasına sahip değilseniz JSON verilerini [içe aktararak](create-new-app.md#import-new-app) [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulama oluşturun. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-hier-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `simple` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar.  

## <a name="purpose-of-the-app"></a>Uygulamanın amacı
Bu uygulama, bir konuşmadaki verileri nasıl çekeceğinizi göstermektedir. Bir sohbet botundan alınmış olan aşağıdaki konuşmalara bir göz atın:

|Konuşma|Ayıklanabilir iş adı|
|:--|:--|
|I want to apply for the new accounting job. (Yeni muhasebe işine başvurmak istiyorum.)|accounting (muhasebe)|
|Please submit my resume for the engineering position. (Lütfen özgeçmişimi mühendislik pozisyonu için değerlendirmek üzere gönderin.)|engineering (mühendislik)|
|Fill out application for job 123456 (123456 numaralı iş için başvuru yapın)|123456|

Bu öğretici, iş adını ayıklamak için yeni bir varlık ekler. 

## <a name="purpose-of-the-simple-entity"></a>Basit varlığın amacı
Bu LUIS uygulamasındaki basit varlığın amacı, LUIS uygulamasına iş adının ne olduğunu ve konuşmanın hangi bölümünde bulunabileceğini öğretmektir. Konuşmanın iş olan bölümü, sözcük seçimine ve konuşma uzunluğuna göre konuşmadan konuşmaya değişiklik gösterebilen iletidir. LUIS uygulamasının tüm amaçlardaki konuşmaların herhangi birinde bulunabilecek işlerin örneklerine ihtiyacı vardır.  

İş adı isim, fiil veya birden fazla kelimeden oluşan bir tümcecik olabileceğinden belirlemek zordur. Örnek:

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

Bu LUIS uygulamasında birden fazla amaçta iş adları bulunmaktadır. LUIS, bu sözcükleri tüm amaçların konuşmalarında etiketleyerek işin ne olduğu ve konuşmaların hangi bölümünde yer aldığı konusunda daha fazla bilgi edinir.

## <a name="create-job-simple-entity"></a>Basit bir iş varlığı oluşturma

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

    [ ![Sağ taraftaki menü çubuğunun en üstünde bulunan Build (Derleme) ifadesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-primary-and-secondary-data/hr-first-image.png)](./media/luis-quickstart-primary-and-secondary-data/hr-first-image.png#lightbox)

2. **Intents** (Amaçlar) sayfasında **ApplyForJob** amacını seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png "'ApplyForJob' amacı vurgulanmış şekilde LUIS ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png#lightbox)

3. `I want to apply for the new accounting job` konuşmasında `accounting` öğesini seçin, açılır menünün en üst kısmına `Job` yazın ve ardından açılır menüden **Create new entity** (Yeni varlık oluştur) girişini seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "'ApplyForJob' amacı seçilmiş ve varlık oluşturma adımları vurgulanmış LUIS ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

4. Açılır pencerede varlığın adını ve türünü doğrulayıp **Done** (Bitti) öğesini seçin.

    ![İş adı ve basit türü görünen basit varlık oluşturma açılır iletişim kutusu](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

5. `Submit resume for engineering position` konuşmasında `engineering` kelimesini İş varlığı olarak etiketleyin. `engineering` kelimesini seçin ve açılır menüden **Job** (İş) girişini seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "Etiketlenen iş varlığı vurgulanmış LUIS ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)

    Tüm konuşmalar etiketlendi ancak beş konuşma, LUIS'e işle ilgili sözcükler ve tümcecikler hakkında eğitim vermek için yeterli değildir. Sayı değeri içeren işler normal ifade varlığı kullandığından daha fazla örneğe ihtiyaçları yoktur. Sözcük veya tümcecik olan işler için en az 15 tane daha örnek gerekir. 

6. Daha fazla konuşma ekleyin ve iş sözcüklerini veya tümceciklerini **Job** (İş) varlığı olarak etiketleyin. Bir işe alma hizmeti için kullanılan iş türleri tüm alanlar için ortaktır. İşlerin belirli bir sektörle ilgili olmasını istiyorsanız iş sözcüklerinin bu durumu yansıtması gerekir. 

    |Konuşma|İş varlığı|
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

## <a name="label-entity-in-example-utterances-for-getjobinformation-intent"></a>GetJobInformation amacı için konuşmalarda varlığı etiketleme
1. Sol menüden **Intents** (Amaçlar) öğesini seçin.

2. Amaç listesinden **GetJobInformation** girişini seçin. 

3. Örnek konuşmalardaki işleri etiketleyin:

    |Konuşma|İş varlığı|
    |:--|:--|
    |Is there any work in databases? (Veritabanı alanında iş var mı?)|veritabanları|
    |Looking for a new situation with responsibilities in accounting (Muhasebe alanında yeni bir iş arayışım mevcut)|accounting (muhasebe)|
    |What positions are available for senior engineers? (Kıdemli mühendisler için uygun olan pozisyonlar hangileri?)|senior engineers (kıdemli mühendisler)|

    Başka örnek konuşmalar da vardır ancak bunlarda işle ilgili sözcük mevcut değildir.

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

Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. Adres çubuğundaki URL'nin sonuna gidip `Here is my c.v. for the programmer job` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `ApplyForJob` konuşmaları döndürmelidir.

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

## <a name="to-boost-signal-add-jobs-phrase-list"></a>Sinyali güçlendirmek için iş tümcecik listesi ekleme
LUIS-Samples Github deposundaki [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) dosyasını açın. Bu listede binin üzerinde iş sözcüğü ve tümceciği vardır. Listede size anlamlı gelen iş sözcüklerini bulun. İstediğiniz sözcükler ve tümcecikler listede değilse ekleyin.

1. LUIS uygulamasının **Build** (Derleme) bölümünde **Improve app performance** (Uygulama performansını geliştir) kısmındaki **Phrase lists** (Tümcecik listeleri) girişini seçin.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png "Phrase lists (Tümcecik listeleri) sol gezinti düğmesi vurgulanmış ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png#lightbox)

2. **Create new phrase list** (Yeni tümcecik listesi oluştur) öğesini seçin. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png "Create new phrase list (Yeni tümcecik listesi oluştur) düğmesi vurgulanmış ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png#lightbox)

3. Yeni tümcecik listesine `Jobs` adını verin ve jobs-phrase-list.csv dosyasındaki listeyi kopyalayıp **Values** (Değerler) metin kutusuna yapıştırın. Enter'a basın. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Create new phrase list (Yeni tümcecik listesi oluştur) iletişim kutusu açılmış ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Tümcecik listesine daha fazla sözcük eklemek istiyorsanız, **Related Values** (İlgili Değerler) girişlerini gözden geçirin ve ilgili olanları ekleyin. 

4. Tümcecik listesini etkinleştirmek için **Save** (Kaydet) öğesini seçin.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Create new phrase list (Yeni tümcecik listesi oluştur) iletişim kutusu ve tümcecik listesindeki değerler kutusunda sözcükler bulunan ekran görüntüsü")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. Tümcecik listesini kullanmak için uygulamayı tekrar [eğitin](#train-the-luis-app) ve [yayımlayın](#publish-the-app-to-get-the-endpoint-URL).

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

## <a name="phrase-lists"></a>Tümcecik listeleri
Tümcecik listesini eklemek, listedeki sözcüklerin sinyalini güçlendirdi ancak tam eşleşme olarak **kullanılmadı**. Tümcecik listesinde ilk sözcüğü `lead` olan birden fazla işin yanı sıra `welder` işi de mevcut ancak `lead welder` işi mevcut değil. İşler için tümcecik listesi eksik olabilir. Düzenli olarak [uç nokta konuşmalarını gözden geçirin](label-suggested-utterances.md) ve bulduğunuz diğer iş sözcüklerini tümcecik listenize ekleyin. Ardından yeniden eğitin ve yeniden yayımlayın.

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Basit bir amaca ve tümcecik listesine sahip olan bu uygulama, doğal dil sorgu varlığını tanımladı ve iş verilerini döndürdü. 

Sohbet botunuz artık iş başvurusu yapma birincil eylemini ve bu eylemin parametrelerinden biri olan başvurulan işi belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak iş bilgilerini üçüncü taraf bir API üzerinden bir İnsan Kaynakları temsilcisine iletebilir. Bot veya çağrı uygulaması için başka programlama seçenekleri varsa LUIS bu görevleri gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Sol üstteki menüden **My apps** (Uygulamalarım) öğesini seçin. Uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş keyphrase varlığı ekleme](luis-quickstart-intent-and-key-phrase.md)