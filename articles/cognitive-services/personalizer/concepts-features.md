---
title: 'Özellikler: Eylem ve bağlam - Personalizer'
titleSuffix: Azure Cognitive Services
description: Personalizer özellikleri, Eylemler ve içeriği hakkındaki bilgileri daha iyi sıralama önerilerde bulunmak için kullanır. Özellikler, çok genel ya da belirli bir öğe için kullanılabilir.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: edjez
ms.openlocfilehash: 94eaeb6e34e74e1a0f1a3958c23cf33b86c4adcd
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620276"
---
# <a name="features-are-information-about-actions-and-context"></a>Eylemler ve bağlamı hakkında bilgi özellikleridir

Uygulamanızı kullanıcılara verilen bir bağlamda göstermelidir öğrenerek Personalizer hizmeti çalışır.

Personalizer kullanan **özellikleri**, hakkında bilgi olan **geçerli bağlam** en iyi şekilde seçmenizi **eylem**. Özellikler, daha yüksek bir ödül elde etmek için kişiselleştirme yardımcı olabilecek düşündüğünüz tüm bilgileri temsil eder. Özellikler, çok genel ya da belirli bir öğe için kullanılabilir. 

Örneğin, olabilir bir **özellik** hakkında:

* _Kullanıcı_ gibi bir `UserID`. 
* _İçeriği_ video olması gibi bir `Documentary`, `Movie`, veya bir `TV Series`, veya bir perakende öğesi mağazada kullanıma sunulan olup olmadığı.
* _Geçerli_ haftanın günü gibi olduğu süreyi dönem.

Personalizer belgenizdeki değil, sınırlandıracak ya da düzeltme eylemleri ve bağlam için gönderebilirsiniz hangi özellikler:

* Bunlara sahip değilseniz, bazı özellikler için diğer ve bazı eylemler için gönderebilirsiniz. Örneğin, TV dizisi filmler yoksa öznitelikleri olabilir.
* Yalnızca bazı kez kullanılabilen bazı özellikler olabilir. Örneğin, bir mobil uygulama bir web sayfasında daha fazla bilgi sağlayabilir. 
* Zaman içinde ekleyin ve bağlam ve özellikleri Kaldır. Personalizer mevcut olan bilgileri öğrenmeye devam eder.
* Bağlam için en az bir özellik olması gerekir. Boş bir bağlamda personalizer desteklemez. Her seferinde yalnızca bir sabit bağlamı gönderirseniz, Personalizer özellikleri ile ilgili eylemler yalnızca sıralamasına eylemi seçer. 
* En iyi herhangi bir zamanda herkes için eylemleri seçin personalizer çalışacaktır.

## <a name="supported-feature-types"></a>Desteklenen özellik türleri

Personalizer dize, sayısal ve boolean türleri özelliklerini destekler.

### <a name="how-choice-of-feature-type-affects-machine-learning-in-personalizer"></a>Özellik türü seçimi Personalizer Machine Learning'de nasıl etkiler?

* **Dizeleri**: Dize türleri için her bir anahtar ve değer birleşimi yeni ağırlıkları Personalizer machine learning modeli oluşturur. 
* **Sayısal**: Sayı orantılı olarak kişiselleştirme sonucu etkiler, sayısal değerleri kullanmanız gerekir. Bu çok bağımlı bir senaryodur. Basitleştirilmiş örnekte örneğin ne zaman bir perakende kişiselleştirme deneyimi NumberOfPetsOwned iki kez veya thrice olarak 1 evcil hayvan sahip kişiselleştirme sonucu etkilemek için 2 veya 3 Evcil Hayvanlar kişilerle isteyebileceğinizden sayısal özelliğini olabilir. Dizeler ve özellik kalite genellikle aralıkları kullanılarak geliştirilebilir gibi sayısal birimlerde temel alır, ancak burada anlamı yaş, sıcaklık veya kişi Height - gibi doğrusal - olmayan özellikler en iyi kodlanır. Örneğin, yaş "Yaş" kodlanmış: "0-5", "Yaş": "6-10", vs.
* **Boole** gibi bunlar hiç gönderilen yüklediniz, "false" Yasası değeriyle gönderilen değerler.

Mevcut olmayan özellikleri istekten atlanmış olabilir. Gönderme özellikleri null bir değerle kaçının, çünkü, varolan olarak ve "null" değerini içeren modeli eğitimindeki işlenir.

## <a name="categorize-features-with-namespaces"></a>Ad alanları özelliklerle kategorilere ayırma

Ad alanları olarak düzenlenmiş özellikler personalizer alır. Ad kullandıysanız, uygulamanızın ve bunların ne olması gerektiğini belirler. Ad alanları, özellikleri benzer bir konu hakkında veya belirli bir kaynaktan geliyor özellikleri gruplandırmak için kullanılır.

Uygulamaları tarafından kullanılan özellik ad alanları örnekleri aşağıda verilmiştir:

* User_Profile_from_CRM
* Zaman
* Mobile_Device_Info
* http_user_agent
* VideoResolution
* UserDeviceInfo
* Hava durumu
* Product_Recommendation_Ratings
* current_time
* NewsArticle_TextAnalytics

Geçerli JSON anahtarlarını oldukları sürece kendi kurallarına özellik ad alanı adı ekleyebilirsiniz.

Aşağıdaki json'da `user`, `state`, ve `device` özellik adları.

JSON nesneleri, iç içe geçmiş JSON nesneleri ve basit özellik değerlerini içerebilir. Yalnızca sayılar dizi öğeleri, bir dizi dahil edilebilir. 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "name":"Doug",
                "latlong": [47.6, -122.1]
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true
            }
        }
    ]
}
```

## <a name="how-to-make-feature-sets-more-effective-for-personalizer"></a>Özellik yapma daha etkili Personalizer için ayarlar.

İyi özellik kümesini Personalizer yüksek ödül artıracak eylemi tahmin etme hakkında bilgi edinin yardımcı olur. 

Bu önerilere uyun Personalizer derece API gönderme özellikleri göz önünde bulundurun:

* Sürücü kişiselleştirme yeterli özellikler mevcuttur. Daha fazla içerik olması gerekiyor tam olarak hedeflenen, daha fazla özellik gereklidir.

* Yeterli özelliklerini farklı vardır *densities*. Bir özellik *yoğun* birçok öğe, birkaç demet gruplanması durumunda. Örneğin, "Uzun" videoları binlerce sınıflandırılabilir (uzun üzerinde 5 dakika) ve "Kısa" (uzun altında 5 dakika). Bu bir *çok yoğun* özelliği. Öte yandan, öğeler aynı binlerce neredeyse hiç bir öğe diğerine aynı değer gerekir "Title" adlı bir öznitelik olabilir. Bir çok olmayan yoğun budur veya *seyrek* özelliği.  

Yüksek yoğunluklu özelliklerine sahip bir öğesinden öğrenme tahmin Personalizer yardımcı olur. Ancak, yalnızca birkaç özellik vardır ve bunlar çok yoğun Personalizer tam olarak seçilecek yalnızca birkaç demet ile içerik hedef çalışacaktır.

### <a name="improve-feature-sets"></a>Özellik kümeleri geliştirin 

Çevrimdışı değerlendirme yaparak kullanıcı davranışı çözümleyebilir. Bu, hangi özellikleri yoğun olarak daha az katkıda bulunuyorsanız ve pozitif ödül katkıda bulunduğunuz görmek için geçmiş veri bakmak sağlar. Hem de daha iyi özellikleri bulmak için uygulamanızın kadar sonuçlarını daha da geliştirmek için Personalizer göndermeye olacaktır ve özellikleri yardımcı olduğumuz görebilirsiniz.

Aşağıdaki bölümlerde bu özellikler için Personalizer gönderilen artırmak için ortak yöntemleridir.

#### <a name="make-features-more-dense"></a>Özellikler daha yoğun bir hale getirir

Daha büyük ve daha fazla veya daha az yoğun yapmak için bunları düzenleyerek, özellik kümeleri artırmak mümkündür.

Örneğin, bir zaman damgası saniye kadar çok seyrek bir özelliğidir. Bunu daha fazla yoğun (etkin) kez sınıflandırma "sabah", "öğle saati", "öğleden sonra", vb. tarafından yapılamadı.


#### <a name="expand-feature-sets-with-extrapolated-information"></a>Özellik kümeleri ortaya çıkabilecek bilgilerle genişletin

Ayrıca, zaten sahip olduğunuz bilgilerinden türetilen keşfedilmemiş özniteliklerin düşünerek daha fazla özellik elde edebilirsiniz. Örneğin, bir kurgusal film listesi kişiselleştirmeyi, olası, bir hafta sonu vs haftanın günü, kullanıcıların farklı bir davranış verilmesini sağlar nedir? "Hafta" veya "iş günü" özniteliği için zaman genişletilemiyor. İlgiyi belirli film türleri kültürel Ulusal tatilleri yönlendiriyor? Örneğin, "Cadılar Bayramı" özniteliği ilgili olduğu yerde yararlı olur. Rainy hava durumu, tercih ettiğiniz bir filmi üzerinde önemli bir etkisi çoğu kişi için döndürmüş olabilir mi? Zaman ve yerde, hava durumu hizmetine bilgi ve bunu ek bir özellik olarak ekleyebilirsiniz sağlayabilir. 

#### <a name="expand-feature-sets-with-artificial-intelligence-and-cognitive-services"></a>Özellik kümeleri yapay zeka ve bilişsel hizmetler ile genişletin

Yapay zeka ve Bilişsel hizmetler çalıştırılmaya hazır Personalizer çok güçlü bir eklemedir olabilir. 

Yapay zeka hizmetlerini kullanarak öğelerinizi ön işleme, büyük olasılıkla kişiselleştirme için ilgili bilgileri otomatik olarak ayıklayabilirsiniz.

Örneğin:

* Bir film dosyası aracılığıyla çalıştırabileceğiniz [Video Indexer](https://azure.microsoft.com/services/media-services/video-indexer/) Sahne öğeleri, metin, yaklaşımını ve diğer birçok öznitelikleri ayıklamak için. Bu öznitelikler daha sonra özgün öğe meta verisi yoktu özellikleri yansıtacak şekilde daha yoğun yapılabilir. 
* Görüntüleri nesne algılama, yaklaşım, vs. yüzleri aracılığıyla çalıştırabilirsiniz.
* Metin bilgileri varlıkları, Bing bilgi grafiğine vb. varlıklarla genişletme yaklaşım, ayıklayarak genişletilmiş.

Birkaç diğer kullanabileceğiniz [Azure Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services)gibi

* [Varlık bağlama](../entitylinking/home.md)
* [Metin Analizi](../text-analytics/overview.md)
* [Duygu tanıma](../emotion/home.md)
* [Görüntü İşleme](../computer-vision/home.md)

## <a name="actions-represent-a-list-of-options"></a>Eylemlerini temsil eden bir seçenek listesi

Her eylem:

* Bir kimliği vardır.
* Özelliklerin bir listesi vardır.
* (Yüzlerce) büyük özelliklerin listesi olabilir ancak ödül alma için katkıda bulunan olmayan funkce odebrat özellik verimliliğini değerlendirme öneririz. 
* Özellikleri **eylemleri** olabilir veya herhangi bir ilişki özellikleriyle olmayabilir **bağlam** Personalizer tarafından kullanılır.
* Bazı eylemler ve diğer eylemler için özellikleri bulunmayabilir. 
* Belirli bir eylem kimliği özellikleri kullanılabilir bir gün olabilir ancak daha sonra kullanılamaz duruma gelir. 

Personalizer'ın makine öğrenimi algoritması, tutarlı özellik kümesi vardır, ancak özellik değişiklikleri zaman içinde ayarlarsanız derece çağrıları başarısız değil daha iyi sonuç verecektir.

50'den fazla eylemleri sıralaması zaman göndermez eylemler. Bunlar her zaman aynı 50 eylemler olabilir veya bunlar değişebilir. Bir e-ticaret uygulaması için 10.000 öğelerinin bir ürün kataloğu varsa, örneğin, öneri veya filtreleme altyapısı müşteri ister ve Personalizer çoğu ödül (örneğin oluşturacağı bulmak için üst 40 belirlemek için kullanabilirsiniz kullanıcının sepete ekler) için geçerli bağlam.


### <a name="examples-of-actions"></a>Eylem örnekleri

Derece API'sine gönderdiğiniz Eylemler ne kişiselleştirmek çalışıyorsunuz bağlıdır.

Bazı örnekler şunlardır:

|Amaç|Eylem|
|--|--|
|Hangi makale haber Web sitesinde vurgulanır kişiselleştirin.|Her eylem bir olası bir haber makaledir.|
|Bir Web sitesinde ad yerleştirme iyileştirin.|Her eylem bir düzen veya reklam (örneğin, en üstte, sağ, küçük resimler büyük görüntüleri üzerinde) için bir düzen oluşturmak için kuralları olacaktır.|
|Önerilen öğeler kişiselleştirilmiş sıralamasını bir alışveriş Web sitesinde görüntüler.|Her bir eylemin belirli bir üründür.|
|Belirli bir fotoğraf uygulanacak filtreler gibi kullanıcı arabirimi öğeleri önerin.|Her eylem, başka bir filtre olabilir.|
|Kullanıcının amacını açıklamak veya bir eylem önermek için bir sohbet Robotu kişinin yanıt'ı seçin.|Her eylem, nasıl yanıt yorumlamak için kullanılan bir seçenektir.|
|Neyin arama sonuçlarının bir listenin en üstünde göstermek seçin|Her eylem üst birkaç arama sonuçlarını biridir.|


### <a name="examples-of-features-for-actions"></a>Eylemler için özellikleri örnekleri

Eylemler için özellikler iyi örnekleri şunlardır: Bu, her bir uygulama üzerinde çok bağlıdır.

* Özellikler eylemlerin özelliklere sahip. Örneğin, film veya tv dizisi mi?
* Özellikler, hakkında nasıl kullanıcıların geçmişte Bu eylemle kurulabilen. Örneğin, bu film çoğunlukla olan demografisi A veya B kişiler tarafından görülen, tipik olarak yürütülen birden fazla vakit geldi.
* Özellikler hakkında nasıl özelliklerini kullanıcı *görür* eylemleri. Örneğin, posteri Küçük Resim Ekle yüzler, arabalarda veya ortamlarını gösterilen film için mu?

### <a name="load-actions-from-the-client-application"></a>İstemci uygulamasından eylemleri yüklenemiyor

Eylemler özelliklerinden genellikle içerik yönetim sistemleri, katalog ve öneren sistemlerinden gelebilir. Uygulamanız, eylemler hakkında bilgi yüklemek için ilgili veritabanları ve sahip olduğunuz sistemlerinden sorumludur. Eylemlerinizi değiştirmeyin veya yüklü alamazsınız her zaman bir gereksiz performans üzerinde etkisi vardır, bu bilgileri önbelleğe almak için uygulamanızda mantığı ekleyebilirsiniz.

### <a name="prevent-actions-from-being-ranked"></a>Sıralanmış gelen eylemleri engelleme

Bazı durumlarda, kullanıcılara görüntülenmesini istemediğiniz eylemler vardır. En üstteki olarak sıralanmış bir eylemin önlemek için en iyi yolu, sıra API için eylem listesinden ilk başta değil eklemektir.

Bazı durumlarda, yalnızca iş mantığınıza daha sonra bir sonuç ise belirlenemez _eylem_ derece API'si, bir kullanıcıya gösterilecek çağrıdır. Bu durumlarda, kullanmanız gereken _etkin olmayan olaylar_.

## <a name="json-format-for-actions"></a>Eylemler için JSON biçimi

Derece çağrılırken, aralarından seçim yapabileceğiniz birden fazla eylem gönderir:

JSON nesneleri, iç içe geçmiş JSON nesneleri ve basit özellik değerlerini içerebilir. Yalnızca sayılar dizi öğeleri, bir dizi dahil edilebilir. 

```json
{
    "actions": [
    {
      "id": "pasta",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "medium",
          "grams": [400,800]
        },
        {
          "nutritionLevel": 5,
          "cuisine": "italian"
        }
      ]
    },
    {
      "id": "ice cream",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [150, 300, 450]
        },
        {
          "nutritionalLevel": 2
        }
      ]
    },
    {
      "id": "juice",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [300, 600, 900]
        },
        {
          "nutritionLevel": 5
        },
        {
          "drink": true
        }
      ]
    },
    {
      "id": "salad",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "low",
          "grams": [300, 600]
        },
        {
          "nutritionLevel": 8
        }
      ]
    }
  ]
}
```

## <a name="examples-of-context-information"></a>Bağlam bilgilerini örnekleri

Bilgi için _bağlam_ her uygulama ve kullanım durumunu temel bağlıdır ancak bilgileri gibi tipik dahil:

* Kullanıcı ile ilgili demografik ve profil bilgileri.
* Kullanıcı Aracısı gibi HTTP üstbilgileri ayıklanan veya IP adreslerine göre geriye doğru coğrafi arama gibi HTTP bilgilerinden türetilen bilgileri.
* Hafta sonu veya değil, sabah veya öğleden sonra mağazalarımızdaki ürünler haftanın günü gibi geçerli saati hakkında bilgi veya değil, vb.
* Mobil uygulamaları, konum, taşıma veya pil düzeyi gibi ayıklanan bilgileri.
* Kullanıcı - film türleri nelerdir gibi davranış geçmiş toplamları bu kullanıcı en çok görüntülenen.

Uygulamanızın bağlamı hakkında bilgi yüklemek için ilgili veritabanları, algılayıcılar ve sistemleri etkinleştirmiş olabilirsiniz sorumludur. Bağlam bilgilerinizi değişmiyorsa, sıra API için göndermeden önce bu bilgileri önbelleğe almak için uygulamanızda mantığı ekleyebilirsiniz.

## <a name="json-format-for-context"></a>Bağlamı için JSON biçimi 

Bağlam derece API'ye gönderilen bir JSON nesnesi olarak ifade edilir:

JSON nesneleri, iç içe geçmiş JSON nesneleri ve basit özellik değerlerini içerebilir. Yalnızca sayılar dizi öğeleri, bir dizi dahil edilebilir. 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "name":"Doug"
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true,
                "screensize": [1680,1050]
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) 
