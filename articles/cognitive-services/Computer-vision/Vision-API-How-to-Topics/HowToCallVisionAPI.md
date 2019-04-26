---
title: "Örnek: Analiz resim API'si - görüntü işleme çağırma"
titlesuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler’de REST’i kullanarak Görüntü İşleme API’sinin nasıl çağrılacağını öğreneceksiniz.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 03/21/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 0e2767660edf2a9dbcb8617b07a6b9f71fedb743
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60202879"
---
# <a name="example-how-to-call-the-computer-vision-api"></a>Örnek: Görüntü işleme API'sini çağırma

Bu kılavuzda, REST kullanılarak Görüntü İşleme API’sinin nasıl çağrılacağı gösterilmektedir. Örnekler hem Görüntü İşleme API’si istemci kitaplığı kullanılarak C# dilinde hem de HTTP POST/GET çağrıları olarak yazılır. Odaklanacaklarımız:

- "Etiketler", "Açıklama" ve "Kategoriler" alma.
- "Etki alanına özgü" bilgileri alma (ünlüler)

## <a name="prerequisites"></a>Önkoşullar

- Yerel olarak depolanan görüntünün yolu veya görüntü URL’si.
- Giriş yöntemleri desteklenir: İkili bir uygulama/octet stream veya resim URL'si biçiminde ham görüntü
- Resim biçimleri desteklenir: JPEG, PNG, GIF, BMP
- Resim dosyasının boyutu: 4 MB'tan az
- Görüntü boyutu: 50 x 50 piksel büyüktür
  
Aşağıdaki örneklerde, aşağıdaki özellikler gösterilmektedir:

1. Bir görüntüyü analiz etme ve döndürülen bir açıklamayı ve etiket dizisini alma.
2. Etki alanına özgü model (özellikle, “ünlüler” modeli) ile bir görüntüyü analiz etme ve JSON döndürmesinde ilgili sonucu alma.

Özellikler şunlara ayrılır:

- **Birinci seçenek:** Kapsamlı analiz - yalnızca belirli bir model analiz edin
- **İki seçenek:** Gelişmiş analiz - ek ayrıntılar ile sağlamak için analiz [86-kategori sınıflandırma](../Category-Taxonomy.md)
  
## <a name="authorize-the-api-call"></a>API çağrısı Yetkilendir

Görüntü İşleme API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtarın bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilmesi gerekir.

Ücretsiz bir deneme sürümü anahtarı almak için bkz: [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.

1. Sorgu dizesi aracılığıyla abonelik anahtarını geçirme, Görüntü İşleme API’si örneği olarak aşağıdaki örneğe bakın:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

1. Abonelik anahtarını geçirme, HTTP isteği üst bilgisinde de belirtilebilir:

```ocp-apim-subscription-key: <Your subscription key>```

1. İstemci kitaplığı kullanılırken, VisionServiceClient sınıfının oluşturucusu aracılığıyla abonelik anahtarı geçirilir:

```var visionClient = new VisionServiceClient("Your subscriptionKey");```

## <a name="upload-an-image-to-the-computer-vision-api-service-and-get-back-tags-descriptions-and-celebrities"></a>Görüntü işleme API'si hizmeti için bir görüntü yükleyin ve etiketler, açıklamalar ve ünlüleri Geri Al

Görüntü İşleme API’si çağrısını gerçekleştirmenin temel yolu, bir görüntünün doğrudan karşıya yüklenmesiyle gerçekleşir. Görüntüden okunan verilerle uygulama/sekizli akış içerik türü ile bir "POST" isteği gönderilerek bu yapılır. "Etiketler" ve "Açıklama" için bu karşıya yükleme yöntemi, tüm Görüntü İşleme API’si çağrıları için aynı olacaktır. Tek fark, kullanıcının belirttiği sorgu parametreleridir. 

Belirli bir görüntü için "Etiketler" ve "Açıklama" alma işlemi şöyledir:

**Birinci seçenek:** "Tags" listesini ve bir "Description" Al

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```

```csharp
using Microsoft.ProjectOxford.Vision;
using Microsoft.ProjectOxford.Vision.Contract;
using System.IO;

AnalysisResult analysisResult;
var features = new VisualFeature[] { VisualFeature.Tags, VisualFeature.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.AnalyzeImageAsync(fs, features);
}
```

**İkinci Seçenek** Yalnızca "Etiketler" listesini veya yalnızca "Açıklama" listesini alma:

###### <a name="tags-only"></a>Yalnızca etiketler:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Yalnızca açıklaması:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```

### <a name="get-domain-specific-analysis-celebrities"></a>Etki alanına özgü analizi (ünlüleri) alın

**Birinci seçenek:** Kapsamlı analiz - yalnızca belirli bir model analiz edin
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```

Bu seçenek için diğer tüm sorgu parametreleri {visualFeatures, details} geçerli değildir. Desteklenen tüm modelleri görmek istiyorsanız şu seçeneği kullanın:

```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```

**İki seçenek:** Gelişmiş analiz - ek ayrıntılar ile sağlamak için analiz [86-kategori sınıflandırma](../Category-Taxonomy.md)

Bir veya daha fazla etki alanına özgü modelde yer alan ayrıntılara ek olarak genel görüntü analizi almak istediğiniz uygulamalar için, modeller sorgu parametresi ile v1 API’sini genişletiriz.

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```

Bu yöntem çağrıldığında önce 86 kategorisi sınıflandırıcısını çağırırız. Herhangi bir kategori, bilinen/eşleşen bir modelin kategorisiyle eşleşiyorsa, ikinci bir sınıflandırıcı çağrısı geçirme işlemi gerçekleşir. Örneğin, "details=all" veya "details" olursa ‘celebrities’ öğesini ekleyin; böylece 86 kategorisi sınıflandırıcısı çağrıldıktan sonra ünlüler modelini çağırırız ve sonuç, kişi kategorisini içerir. Bu, ünlülerle ilgilenen kullanıcılar için Birinci Seçeneğe kıyasla gecikme süresini artırır.

Tüm v1 sorgu parametreleri, bu durumda aynı şekilde davranır.  visualFeatures=categories belirtilmezse, örtük olarak etkinleştirilir.

## <a name="retrieve-and-understand-the-json-output-for-analysis"></a>Alma ve analiz için JSON çıkışını anlama

Bir örneği aşağıda verilmiştir:

```json
{  
  "tags":[  
    {  
      "name":"outdoor",
      "score":0.976
    },
    {  
      "name":"bird",
      "score":0.95
    }
  ],
  "description":{  
    "tags":[  
      "outdoor",
      "bird"
    ],
    "captions":[  
      {  
        "text":"partridge in a pear tree",
        "confidence":0.96
      }
    ]
  }
}
```

Alan | Tür | İçerik
------|------|------|
Etiketler  | `object` | Etiket dizisi için üst düzey nesnedir
tags[].Name | `string`  | Etiketler sınıflandırıcısındaki anahtar sözcüktür
tags[].Score    | `number`  | 0 ile 1 arasında güven puanıdır
açıklama  | `object` | Açıklama için üst düzey nesnedir.
description.tags[] |    `string`    | Etiketlerin listesidir.  Açıklamalı alt yazı üretme özelliği yeterince güvenilir değilse, çağıranın kullanımına sunulan tek bilgi etiketler olabilir.
description.captions[].text | `string`  | Görüntüyü açıklayan bir ifadedir.
description.captions[].confidence   | `number`  | İfade için güven düzeyidir.

## <a name="retrieve-and-understand-the-json-output-of-domain-specific-models"></a>Alma ve alana özgü modeller JSON çıkışını anlama

**Birinci seçenek:** Kapsamlı analiz - yalnızca belirli bir model analiz edin

Çıkış bir etiket dizisi olacaktır; örnek şuna benzer:

```json
{  
  "result":[  
    {  
      "name":"golden retriever",
      "score":0.98
    },
    {  
      "name":"Labrador retriever",
      "score":0.78
    }
  ]
}
```

**İki seçenek:** Gelişmiş analiz - 86 kategorileri sınıflandırma ile ek ayrıntı sağlamak için analiz edin

İkinci Seçeneği (Gelişmiş Analiz) kullanan etki alanına özgü modeller için kategoriler döndürme türü genişletilmiştir. Aşağıda bir örnek verilmiştir:

```json
{  
  "requestId":"87e44580-925a-49c8-b661-d1c54d1b83b5",
  "metadata":{  
    "width":640,
    "height":430,
    "format":"Jpeg"
  },
  "result":{  
    "celebrities":[  
      {  
        "name":"Richard Nixon",
        "faceRectangle":{  
          "left":107,
          "top":98,
          "width":165,
          "height":165
        },
        "confidence":0.9999827
      }
    ]
  }
}
```

Kategoriler alanı, özgün taksonomideki [86 kategorisinden](../Category-Taxonomy.md) birinin veya daha fazlasının listesidir. Alt çizgiyle biten kategorilerin, bu kategori ve alt öğeleri ile eşleşeceğini de unutmayın (örneğin, ünlüler modeli için people_ ve people_group).

Alan   | Tür  | İçerik
------|------|------|
kategoriler | `object`   | Üst düzey nesne
categories[].name    | `string` | 86 kategorisi sınıflandırmasındaki ad
categories[].score  | `number`  | 0 ile 1 arasında güven puanı
categories[].detail  | `object?`      | İsteğe bağlı ayrıntı nesnesi

Birden fazla kategori eşleşiyorsa (örneğin, model=ünlüler olduğunda 86 kategorisi sınıflandırıcısı, hem people_ hem de people_young için bir puan döndürür), ayrıntılar en genel düzey eşleşmesine (bu örnekte people_) eklenir.

## <a name="errors-responses"></a>Hata yanıtları

Bunlar, hem Option One hem de Option Two senaryosunda döndürülebilecek ek NotSupportedModel hatası (HTTP 400) ile vision.analyze öğesine benzer. İkinci Seçenek (Gelişmiş Analiz) için, ayrıntılarda belirtilen modellerden herhangi biri tanınmıyorsa, bunlardan biri veya daha fazlası geçerli olsa da API bir NotSupportedModel döndürür.  Kullanıcılar, hangi modellerin desteklendiğini öğrenmek için listModels öğesini çağırabilir.

## <a name="next-steps"></a>Sonraki adımlar

REST API’yi kullanmak için [Görüntü İşleme API’si Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44) bölümüne gidin.
