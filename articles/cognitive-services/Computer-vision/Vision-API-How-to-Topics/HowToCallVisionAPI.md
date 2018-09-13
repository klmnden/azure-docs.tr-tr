---
title: Görüntü işleme API'si çağırma | Microsoft Docs
description: Bilişsel Hizmetler'e REST kullanarak görüntü işleme API'sini çağırma hakkında bilgi edinin.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 01/20/2017
ms.author: kefre
ms.openlocfilehash: 34705681e665b57a89c43f31ca695e0acb45ae3f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35760279"
---
# <a name="how-to-call-the-computer-vision-api"></a>Görüntü işleme API'sini çağırma

Bu kılavuz, bilgisayar işleme kullanarak REST API'nin nasıl çağrılacağını gösterir. Örnekler, hem C# görüntü işleme API'si istemci kitaplığı kullanılarak ve HTTP POST/GET çağrıları olarak yazılır. Odaklanacağız:

-   "Tags", "Description" ve "Kategoriler" almak nasıl.
-   (Ünlüleri) "Etki alanına özgü" bilgi edinme.

### <a name="Prerequisites">Önkoşullar</a> 
Resim URL'si veya yerel olarak depolanan bir resmi yolu.
  * Giriş yöntemleri desteklenir: ikili bir uygulama/octet stream veya resim URL'si biçiminde ham görüntü
  * Desteklenen görüntü biçimleri: JPEG, PNG, GIF, BMP
  * Görüntü dosyası boyutu: 4 MB'tan az
  * Görüntü boyutu: 50 x 50 piksel büyüktür
  
Aşağıdaki örneklerde, aşağıdaki özellikleri gösterilmiştir:

1. Görüntü analizi ve bir dizi etiketlerinin ve bir açıklama alma döndürdü.
2. Bir etki alanına özgü modeli (özellikle, model "ünlüleri") ile görüntü analiz etme ve ilgili alma JSON retune neden.

Özellikler üzerinde ayrılmıştır:

  * **Bir seçenek:** kapsamındaki analizi - yalnızca belirli bir model analiz edin
  * **İki seçenek:** Gelişmiş analiz - ek ayrıntılar ile sağlamak için analiz [86-kategori sınıflandırma](../Category-Taxonomy.md)
  
### <a name="Step1">1. adım: API çağrısı yetkilendirme</a> 
Görüntü işleme API'si yapılan her çağrı bir abonelik anahtarı gerektirir. Bu anahtarın bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilmesi gerekir. 

Bir abonelik anahtarı edinmek için bkz: [abonelik anahtarlarını almak için nasıl](../Vision-API-How-to-Topics/HowToSubscribe.md
).

**1.** Sorgu dizesi aracılığıyla abonelik anahtarını geçirerek, aşağıda bir görüntü işleme API'si örnek olarak bkz:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

**2.** Abonelik anahtarını geçirerek HTTP istek bağlığında belirtilebilir:

```ocp-apim-subscription-key: <Your subscription key>```

**3.** İstemci kitaplığını kullanarak, abonelik anahtarını VisionServiceClient Oluşturucu üzerinden geçirilir:

```var visionClient = new VisionServiceClient(“Your subscriptionKey”);```

### <a name="Step2">2. adım: görüntü işleme API'si hizmeti için bir görüntü yükleyin ve etiketler, açıklamalar ve ünlüleri Geri Al</a>
Görüntü işleme API'si çağrısı gerçekleştirmek için temel görüntüyü karşıya yükleyerek doğrudan yoludur. Bu, uygulama/octet-akış içerik türüyle görüntüden okuma verileriyle birlikte bir "POST" istek göndererek gerçekleştirilir. "Tags" ve "Description" için bu yükleme yöntemi için tüm görüntü işleme API'si çağrılarının aynı olacaktır. Tek fark, kullanıcının belirttiği sorgu parametreleri olacaktır. 

Belirli bir görüntü için "Tags" ve "Description" alma şöyledir:

**Bir seçenek:** "Tags" listesini ve bir "Description" Al
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```
```
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
**İki seçenek** yalnızca Get listesi "Etiket" veya "Description" yalnızca listesi:

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
### <a name="here-is-how-to-get-domain-specific-analysis-in-our-case-for-celebrities"></a>(Bu örnekte, ünlüleri için) etki alanına özgü analiz almak açıklanmıştır.

**Bir seçenek:** kapsamındaki analizi - yalnızca belirli bir model analiz edin
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```
Bu seçenek için diğer tüm sorgu parametreleri {visualFeatures, ayrıntılar} geçerli değildir. Desteklenen tüm modelleri görmek istiyorsanız, bu seçeneği kullanın: 
```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```
**İki seçenek:** Gelişmiş analiz - ek ayrıntılar ile sağlamak için analiz [86-kategori sınıflandırma](../Category-Taxonomy.md)

Bir veya daha fazla alana özgü modeller genel görüntü analizi ek ayrıntıları almak için istediğiniz uygulamalar için biz v1 API modelleri sorgu parametresi ile genişletin.
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```
Bu yöntem çağrıldığında, biz 86 kategori sınıflandırıcı çağıracaktır. Kategoriler, bilinen ve eşleşen bir modelin hiçbiriyle ikinci bir sınıflandırıcı çağrılarını geçişi meydana gelir. Örneğin, "Ayrıntılar = all", veya "details" 'ünlüleri' dahil, biz ünlüleri modeli 86 kategori Sınıflandırıcısı olarak adlandırılır ve kategori kişi sonucu içeren sonra çağırır. Bu seçenek için bir karşılaştırma ünlüleri, ilginizi kullanıcılar için gecikme süresini artırır.

Tüm v1 sorgu parametreleri, bu durumda aynı davranır.  Varsa visualFeatures = kategoriler belirtilmezse, örtük olarak etkin olacaktır.

### <a name="Step3">3. adım: Alma ve analiz & visualFeatures JSON çıkışını anlama etiketler, açıklama =</a>

Bir örneği aşağıda verilmiştir:
```
  {
    “tags”: [
    {
    "name": "outdoor",
        "score": 0.976
    },
    {
    "name": "bird",
        "score": 0.95
    }
            ],
    “description”: 
    {
    "tags": [
    "outdoor",
    "bird"
            ],
    "captions": [
    {
    "text”: “partridge in a pear tree”,
            “confidence”: 0.96
    }
                ]
    }
  }
```
Alan   | Tür  | İçerik
------|------|------|
Etiketler    | object    | Etiketler dizisi için üst düzey nesnesi
[] etiketler. Adı | dize    | Etiketleri sınıflandırıcı from anahtar sözcüğü
[] etiketler. Puan    | number    | 0 ile 1 arasında güven puanı.
açıklama  | object   | Bir açıklama için üst düzey nesnesi.
Description.Tags] |    dize  | Etiket listesi.  Varsa bir başlık, etiketler belki de yalnızca bilgileri çağırana kullanılabilir üretmek için özelliği var. yetersiz ellerde.
Description.Captions[].Text | dize    | Görüntüyü tanımlayan bir ifade.
Description.Captions[].confidence   | number    | Güvenle deyimi.

### <a name="Step4">4. adım: Alma ve alana özgü modeller JSON çıkışını anlama</a>

**Bir seçenek:** kapsamındaki analizi - yalnızca belirli bir model analiz edin

Çıktı bir etiketler dizisi, bir örnek şu örnekteki gibi olacaktır:
```
  { 
    "result": [ 
      { 
    "name": "golden retriever", 
    "score": 0.98
    },
    { 
    "name": "Labrador retriever", 
    "score": 0.78
    }
              ]
  }
```

**İki seçenek:** Gelişmiş analiz - 86 kategorileri sınıflandırma ile ek ayrıntı sağlamak için analiz edin

İki seçenek (Gelişmiş analiz) kullanarak etki alanına özgü modeller, genişletilmiş türü kategorileri döndürür. Bir örnek aşağıda verilmiştir:
```
  {
    "requestId": "87e44580-925a-49c8-b661-d1c54d1b83b5",
    "metadata":     {
      "width": 640,
      "height": 430,
      "format": "Jpeg"
                    },
    "result": {
      "celebrities": 
      [
        {
        "name": "Richard Nixon",
        "faceRectangle": {
          "left": 107,
          "top": 98,
          "width": 165,
          "height": 165
                         },
        "confidence": 0.9999827
        }
      ]
  }
```

Kategoriler alanı en az biri bir listesidir [86-kategorileri](../Category-Taxonomy.md) özgün taksonomide. Ayrıca, kategoriler alt çizgi bitiş kategori ve alt öğeleri (örneğin, people_ yanı sıra, ünlüleri modeli için people_group) eşleşen unutmayın.

Alan   | Tür  | İçerik
------|------|------|
kategoriler | object | Üst düzey nesnesi
Kategoriler [] .name    | dize   | Ad 86 kategori sınıflandırma
Kategoriler [] .score  | number    | 0 ile 1 arasında bir güven puanı
Kategoriler [] .detail  | Nesne?      | İsteğe bağlı bir ayrıntı nesnesi

Birden çok kategori eşleşiyorsa unutmayın (örneğin, 86 kategori sınıflandırıcı people_ hem people_young model, bir puan döndürür ünlüleri =), ayrıntı düzeyi en genel eşleştir (Bu örnekte people_.) eklenmiş

### <a name="Errors">Hata yanıtları</a>
Bunlar vision.analyze için bir seçenek ve seçeneği iki senaryolarda döndürülen NotSupportedModel hatası (HTTP 400), ek hata ile aynıdır. Ayrıntılarında belirtilen modelleri tanınmıyorsunuz olduğunuz bir veya daha fazlası geçersiz bile seçeneği iki için (Gelişmiş analiz), API bir NotSupportedModel döndürür.  Kullanıcılar hangi modelleri desteklendiğini öğrenmek için listModels çağırabilirsiniz.

### <a name="Summary">Özet</a>

Görüntü işleme API'si, temel işlevler şunlardır: nasıl görüntüleri karşıya yüklemek ve değerli meta verileri iade alma.

REST API'yi kullanmak için Git [bilgisayar görüntü işleme API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
 
