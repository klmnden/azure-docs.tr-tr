---
title: V2 API'si tarafından üretilen Azure Video dizin oluşturucu çıktıyı inceleyin | Microsoft Docs
description: Bu konuda v2 API'si tarafından üretilen Video dizin oluşturucu çıkış inceler.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 6c410b054ba98961d15a4db0ff7eaa2804245cb0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355871"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>V2 API'si tarafından üretilen Video dizin oluşturucu çıktıyı inceleyin

> [!Note]
> Video dizin oluşturucu V1 API'leri artık kullanımdan kaldırıldı ve 1 Ağustos 2018 kaldırılacak. Kesintilerini önlemek için Video dizin oluşturucu v2 API'leri kullanarak başlamanız gerekir.
>
> Video dizin oluşturucu v2 API'leri ile geliştirmek için lütfen bulunan yönergeleri başvurun [burada](https://api-portal.videoindexer.ai/). 

Çağırdığınızda **alma Video dizin** API ve yanıt durumu Tamam, ayrıntılı bir JSON çıkış yanıt içeriği olarak alın. JSON içeriği belirtilen video Öngörüler ayrıntılarını içerir. Boyutlar gibi bilgileri içerir: dökümleri, karakterlerini, yüzler, konular, blokları, vb. Boyutlar her boyut videoda görüntülendiğinde gösteren zaman aralıklarına örnekleri sahip.  

Video özetlenen Öngörüler tuşlarına basarak da görsel olarak inceleyebilirsiniz **Yürüt** Video dizin oluşturucu portal videoda düğmesinde. Daha fazla bilgi için bkz: [görüntüleme ve düzenleme video Öngörüler](video-indexer-view-edit.md).

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Bu makalede tarafından döndürülen JSON içeriği inceler **alma Video dizin** API. 

> [!NOTE]
> Video Oluşturucudaki tüm erişim belirteçleri sona erme tarihi bir saattir.


## <a name="root-elements"></a>Kök öğe

|Ad|Açıklama|
|---|---|
|Hesap Kimliği|Çalma 's VI hesap kimliği.|
|id|Çalma kullanıcının kimliği.|
|ad|Çalma listesi'nın adı.|
|açıklama|Çalma listesi'nın açıklaması.|
|Kullanıcı adı|Çalma oluşturan kullanıcının adı.|
|oluşturuldu|Çalma 's oluşturulma zamanı.|
|privacyMode|Çalma ait gizlilik modu (özel/ortak).|
|durum|Çalma listesi'nın (karşıya yüklenen, işleme, işlenen, başarısız, karantinaya alınan).|
|isOwned|Olup çalma geçerli kullanıcı tarafından oluşturuldu.|
|Iseditable|Çalma düzenlemek için geçerli kullanıcının yetkili olup olmadığı.|
|isBase|Çalma temel çalma listesi (video) veya (türetilmiş) diğer videoları yapılan bir çalma listesi olup olmadığı.|
|durationInSeconds|Toplam süre çalma listesi.|
|summarizedInsights|İçeriyor [summarizedInsights](#summarizedinsights).
|videolar|Listesini [videolar](#videos) çalma listesi oluşturma.<br/>Bu listedeki videoları (türetilmiş), diğer videolar zaman aralıklarına bu çalma listesini oluşturulan, yalnızca verileri dahil zaman aralığından içerir.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

Bu bölümde Öngörüler özetini gösterir.

|Öznitelik | Açıklama|
|---|---|
|ad|Görüntü adı. Örneğin, Azure İzleyicisi.|
|shortId|Video kimliği. Örneğin, 63c6d532ff.|
|privacyMode|Çözümleme aşağıdaki modlarından birini içerebilir: **özel**, **ortak**. **Ortak** -video herkesin hesabınızı ve videoya bir bağlantı olan herkes tarafından da görülebilir. **Özel** -video hesabınızda herkes tarafından da görülebilir.|
|süre|Bir öngörü zamanını açıklayan bir süresini içerir. Süresi geçen saniye cinsinden süredir.|
|thumbnailUrl|Video küçük resim tam URL. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO..". Video özel ise, URL bir saat erişim belirteci içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [accessToken]').|
|yazıtipleri|Bir veya daha fazla yüzeyleri içerebilir. Daha fazla bilgi için bkz: [yüzeyleri](#faces).|
|Konuları|Bir veya daha fazla konuları içerebilir. Daha fazla bilgi için bkz: [konuları](#topics).|
|yaklaşımlar|Bir veya daha fazla düşüncelerin içerebilir. Daha fazla bilgi için bkz: [düşüncelerin](#sentiments).|
|audioEffects| Bir veya daha fazla audioEffects içerebilir. Daha fazla bilgi için bkz: [audioEffects](#audioeffects).|
|markalar| Sıfır veya daha fazla markalar içerebilir. Daha fazla bilgi için bkz: [markalar](#brands).|
|İstatistikleri | Daha fazla bilgi için bkz: [istatistikleri](#statistics).|

### <a name="statistics"></a>İstatistikleri

|Ad|Açıklama|
|---|---|
|CorrespondenceCount|Video yazışmalar sayısı.|
|WordCount|Konuşmacı başına sözcükler sayısı.|
|SpeakerNumberOfFragments|Bir video Konuşmacı sahip parçaları miktarı.|
|SpeakerLongestMonolog|Konuşmacı uzun monolog. Konuşmacı monolog içinde silences varsa dahil edilir. Sessizlik başına ve monolog sonundaki kaldırılır.| 
|SpeakerTalkToListenRatio|Hesaplama video toplam zamanına göre bölünmüş Konuşmacı monolog (olmadan arasındaki sessizlik) üzerinde harcanan zamanı temel alır. Zaman üçüncü ondalık noktasına yuvarlanır.|

## <a name="videos"></a>videolar

|Ad|Açıklama|
|---|---|
|Hesap Kimliği|Video VI hesap kimliği.|
|id|Video kimliği.|
|ad|Video adı.
|durum|Video durumu (karşıya yüklenen, işleme, işlenen, başarısız, karantinaya alınan).|
|processingProgress|(Örneğin, % 20) işlemi sırasında işleme devam ediyor.|
|failureCode|İşlem (örneğin, ' UnsupportedFileType') için başarısız olursa hata kodu.|
|failureMessage|İşlemi başarısız oldu, hata iletisi.|
|externalId|(Kullanıcı tarafından belirtilen değilse) video dış kimliği.|
|Dış URL'sinin gösterilmesini|(Kullanıcı tarafından belirtilen değilse) video dış url.|
|meta veriler|(Kullanıcı tarafından belirtilen değilse) video dış meta veriler.|
|isAdult|Video el ile olup gözden geçirdikten ve bir yetişkin video tanımlanır.|
|görüşler|Öngörüler nesnesi.|
|thumbnailUrl|Video küçük resim tam URL. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO..". Video özel ise, URL bir saat erişim belirteci içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [accessToken]').|
|publishedUrl|Video akışını sağlamak için bir url.|
|publishedUrlProxy|(İçin Apple aygıtlar) video akışını sağlamak için bir url.|
|viewToken|Video akışı için bir kısa süreli görünüm belirteci.|
|sourceLanguage|Video kaynak dili.|
|Dil|Video gerçek dili (çevirisi).|
|indexingPreset|Video dizin oluşturmak için kullanılan hazır.|
|streamingPreset|Video yayımlamak için kullanılan hazır.|
|linguisticModelId|Video transcribe için kullanılan CRI modeli.|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>görüşler

Öngörüler (örneğin, dökümü satırları, yüzler, markalar, vb.), burada her boyut (örneğin, face1, face2, Yüz3) benzersiz öğelerinin bir listesini görmek ve kendi meta verileri ve onun örneklerinin bir listesini her öğeye sahip boyutları kümesidir (olduğu zaman aralığı ile ek isteğe bağlı meta veriler).

Bir nominal bir kimliği, bir ad, bir küçük resim, diğer meta verileri ve zamana bağlı örneklerinin bir listesini olabilir (örneğin: 00:00:05: 00:00:10, 00:01:00 - 00:02:30 ve 00:41:21: 00:41:49.) Zamana bağlı her örneği ek meta veri olabilir. Örneğin, yüz 's dikdörtgen (20,230,60,60) düzenler.

|Sürüm|Kod sürümü|
|---|---|
|sourceLanguage|Video kaynak dili (bir ana dil varsayılarak). Biçiminde bir [BCP 47](https://tools.ietf.org/html/bcp47) dize.|
|Dil|(Kaynak dilden çevrilmesi) ınsights dili. Biçiminde bir [BCP 47](https://tools.ietf.org/html/bcp47) dize.|
|dökümü|[Dökümü](#transcript) boyut.|
|OCR|[Ocr](#ocr) boyut.|
|anahtar sözcükler|[Anahtar sözcükleri](#keywords) boyut.|
|yazıtipleri|[Yüzeyleri](#faces) boyut.|
|Etiketleri|[Etiketleri](#labels) boyut.|
|görüntüleri|[Görüntüleri](#shots) boyut.|
|markalar|[Markalar](#brands) boyut.|
|audioEffects|[AudioEffects](#audioEffects) boyut.|
|yaklaşımlar|[Düşüncelerin](#sentiments) boyut.|
|visualContentModeration|[VisualContentModeration](#visualcontentmoderation) boyut.|
|textualConentModeration|[TextualConentModeration](#textualconentmoderation) boyut.|

Örnek:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualConentModeration": ...
}
```

#### <a name="transcript"></a>dökümü

|Ad|Açıklama|
|---|---|
|id|Satır kimliği.|
|metin|Dökümü kendisi.|
|Dil|Dökümü dili. Dökümü desteklemek her bir satır farklı bir dil sahip olduğu amaçlanmıştır.|
|örnekler|Burada bu satırı görünen zaman aralıkları listesi. Dökümü örneğiyse yalnızca 1 örneği gerekir.|

Örnek:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>OCR

|Ad|Açıklama|
|---|---|
|id|OCR satır kimliği|
|metin|OCR metin.|
|Güven|Tanıma güven.|
|Dil|OCR dili.|
|örnekler|Burada bu OCR görünen zaman aralıkları listesi (aynı OCR birden çok kez görünebilir).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>anahtar sözcükler

|Ad|Açıklama|
|---|---|
|id|Anahtar sözcük kimliği.|
|metin|Anahtar sözcüğü metin.|
|Güven|Anahtar sözcüğü'nın tanıma güven.|
|Dil|(Çevrildiğinde) anahtar sözcüğü dili.|
|örnekler|Bu anahtar sözcük burada görünen zaman aralıkları listesi (bir anahtar sözcük birden çok kez görünebilir).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

#### <a name="faces"></a>yazıtipleri

|Ad|Açıklama|
|---|---|
|id|Yüz kimliği.|
|ad|Yüz adı. 'Bilinmeyen #0', tanımlanan ünlülerle veya müşteri eğitilen kişi olabilir.|
|Güven|Yüz kimliği güven.|
|açıklama|Bir ünlülerle varsa açıklamasını olabilir: "Satya Nadella doğacak adresindeki...". |
|thumbnalId|Bu yüz küçük resim kimliği.|
|knownPersonId|Bilinen bir kişi, iç kimliğini ise|
|Referenceıd|Bir Bing ünlülerle Bing kimliği ise|
|referenceType|Şu anda yalnızca Bing.|
|başlık|Bir ünlülerle başlığını (örneğin "CEO Microsoft'un") ise.|
|ImageUrl|Resim URL'si bir ünlülerle ise.|
|örnekler|Örnekleri bunlar burada yüz belirli bir zaman aralığı içinde görünen biri. Her örnek, ayrıca bir thumbnailsId sahiptir. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>Etiketleri

|Ad|Açıklama|
|---|---|
|id|Etiket kimliği.|
|ad|Etiket adı (örneğin, 'Bilgisayar', 'TV').|
|Dil|Etiket adı (çevrildiğinde) dili. BCP 47|
|örnekler|Bu etiket burada görünen zaman aralıkları listesi (bir etiket birden çok kez görünebilir). Her örneğinin güvenirlik alan vardır. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="shots"></a>görüntüleri

|Ad|Açıklama|
|---|---|
|id|Görüntüsü kimliği.|
|ana kare|(Her bir kimlik ve örnekleri zaman aralıkları listesi vardır) görüntüsü içindeki anahtar çerçeveler listesi. Anahtar çerçeveler örnekleri kimliği kareyi kişinin küçük bir thumbnailId alana sahip|
|örnekler|Bu görüntüsü zaman aralıkları listesi (yalnızca 1 örneği olan görüntüleri).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
       "thumbnailId": "00000000-0000-0000-0000-000000000000",   
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",        
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

#### <a name="brands"></a>markalar

İş ve ürün markasını konuşma metin dökümü ve/veya Video OCR algılandı adları. Bu görsel tanıma markalar veya logosu algılama içermez.

|Ad|Açıklama|
|---|---|
|id|Marka kimliği.|
|ad|Markalar adı.|
|wikiId | Marka wikipedia url soneki. Örneğin, "Target_Corporation" soneki eklenir [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|wikiUrl | Marka, Wikipedia url, kullanıcının bulunmaktadır. Örneğin, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|açıklama|Markalar açıklaması.|
|etiketler|Bu marka ile ilişkili önceden tanımlanmış Etiketler listesi.|
|Güven|Video dizin oluşturucu marka algılayıcısı (0-1) güvenirlik değeri.|
|örnekler|Bu marka zaman aralıkları listesi. Her örneği bu marka dökümü veya OCR görünen olup olmadığını belirten bir brandType sahiptir.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "wikiId": "Microsoft",
    "wikiUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="audioeffects"></a>audioEffects

|Ad|Açıklama|
|---|---|
|id|Ses efekti kimliği.|
|type|Ses efekti türü (örneğin, Clapping, okuma, sessizlik).|
|örnekler|Burada bu ses efekti görünen zaman aralıkları listesi.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>yaklaşımlar

Düşüncelerin sentimentType alanı (nötr/olumlu/olumsuz) tarafından toplanır. Örneğin, 0-0.1, 0,1 0.2.

|Ad|Açıklama|
|---|---|
|id|Düşünceleri kimliği.|
|Ortalama Not karşılaştırması |Bu düşünceleri türü - nötr/olumlu/olumsuz tüm örneklerinin tüm puanları ortalaması|
|örnekler|Burada bu düşünceleri görünen zaman aralıkları listesi.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

VisualContentModeration bloğu olası yetişkinlere yönelik içeriğe sahip Video dizin oluşturucu bulunan zaman aralıklarına içerir. VisualContentModeration boşsa, tanımlanan yetişkin içeriği bulunmamaktadır.

Yetişkin veya saldırganlardan içerik bulunan videolar yalnızca özel görünüm için kullanılabilir. Kullanıcıların durumda IsAdult özniteliği İnsan incelemesi sonucuna içerecek içerik, İnsan gözden geçirmek için bir istek göndermeniz seçeneğiniz vardır.

|Ad|Açıklama|
|---|---|
|id|Görsel içerik yönetimini kimliği.|
|adultScore|Yetişkin puan (içerik denetleyici).|
|racyScore|Saldırganlardan puan (içerik yönetimini).|
|örnekler|Bu görsel içerik yönetimini burada görünen zaman aralıkları listesi.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualconentmoderation"></a>textualConentModeration 

|Ad|Açıklama|
|---|---|
|id|Metin içeriği denetleme kimliği.|
|bannedWordsCount |Engellenenler sözcükler sayısı.|
|bannedWordsRatio |Sözcükler toplam sayısından oranı.|


## <a name="next-steps"></a>Sonraki adımlar

[Video dizin oluşturucu API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

Uygulamanızda pencere öğeleri ekleme hakkında daha fazla bilgi için bkz: [katıştırmak Video dizin oluşturucu widgets, uygulamalara](video-indexer-embed-widgets.md). 

