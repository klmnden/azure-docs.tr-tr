---
title: V2 API'si tarafından üretilen Azure Video dizinleyici çıktısını İnceleme | Microsoft Docs
description: Bu konuda, v2 API'si tarafından üretilen Video dizinleyici çıktısını inceler.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: e4f09e90c1ebb14cdbd528b34e016001c6556540
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389659"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>V2 API'si tarafından üretilen Video dizinleyici çıktısını İnceleme

> [!Note]
> Video Indexer V1 API, 1 Ağustos 2018'de kullanım dışı bırakıldı. Artık, Video Indexer v2 API'si kullanmanız gerekir. <br/>Video Indexer v2 API'leri ile geliştirme için lütfen bulunan yönergeleri okuyun [burada](https://api-portal.videoindexer.ai/). 

Çağırdığınızda **alma Video dizini** API ve yanıt durumunu Tamam, yanıt içeriği olarak ayrıntılı bir JSON çıktısını alın. JSON içeriği belirtilen video öngörüleri ayrıntılarını içerir. Insights gibi boyutları içerir: dökümleri, karakterlerini, yüzleri, konular, blokları vb. Boyutları, her boyut bir videoda özelliğiyken gösteren zaman aralıklarının örneğe sahip.  

Videonun özetlenen ınsights tuşlarına basarak da görsel olarak inceleyebilirsiniz **Play** Video Indexer portalı videoda düğmesine. Daha fazla bilgi için [görüntüleme ve düzenleme video öngörüleri](video-indexer-view-edit.md).

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Bu makalede tarafından döndürülen JSON içeriği inceler **alma Video dizini** API. 

> [!NOTE]
> Video Indexer tüm erişim belirteçlerinde süresi bir saattir.


## <a name="root-elements"></a>Kök öğe

|Ad|Açıklama|
|---|---|
|Hesap Kimliği|Çalma listesi'nın VI hesap kimliği.|
|id|Çalma listesi'nın kimliği.|
|ad|Çalma listesi'nın adı.|
|açıklama|Çalma listesi'nın açıklaması.|
|Kullanıcı adı|Çalma listesini oluşturan kullanıcının adı.|
|oluşturuldu|Çalma listesi'nın oluşturma zamanı.|
|privacyMode|Çalma listesi'nın Gizlilik modu (Private/Public).|
|durum|Çalma listesi'nın (karşıya yüklenen, işleme, işlenen, başarısız, karantinaya alınmış).|
|isOwned|Çalma listesi geçerli kullanıcı tarafından oluşturulmuş olup olmadığını gösterir.|
|Iseditable|Geçerli kullanıcının çalma düzenlemek için yetki verilip verilmediğini belirtir.|
|isBase|Çalma listesi temel bir çalma listesi (video) veya bir çalma listesi (türetilmiş) diğer videoları yapılan olup olmadığını gösterir.|
|Durationınseconds|Çalma listesi toplam süresi.|
|summarizedInsights|Bir tane var. [summarizedInsights](#summarizedinsights).
|video|Listesini [videoları](#videos) çalma listesi oluşturmak.<br/>Bu çalma listesini bu listedeki videoları (türetilmiş), diğer videoları zaman aralıklarının oluşturulan, yalnızca dahil edilen zaman aralıkları verilerini içerir.|

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

Bu bölümde, içgörüler özetini gösterir.

|Öznitelik | Açıklama|
|---|---|
|ad|Videonun adı. Örneğin, Azure İzleyici.|
|shortId|Video kimliği. Örneğin, 63c6d532ff.|
|privacyMode|Döküm şu modlardan birine sahip olabilir: **özel**, **genel**. **Genel** -video herkes hesabınızı ve videoya bir bağlantı olan herkes tarafından görülebilir. **Özel** -video hesabınızdaki herkes tarafından da görülebilir.|
|süre|Bir öngörü gerçekleştiği zaman açıklayan bir süresini içerir. Saniyeler içinde süresidir.|
|thumbnailUrl|Video küçük resmi tam URL'si. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO..". Videonun özel ise, bir saat erişim belirteci URL'sini içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [accessToken]').|
|yüzleri|Sıfır veya daha fazla yüzleri içerebilir. Daha ayrıntılı bilgi için bkz. [yüzleri](#faces).|
|anahtar sözcükler|Sıfır veya daha fazla anahtar sözcükler içerebilir. Daha ayrıntılı bilgi için bkz. [anahtar sözcükleri](#keywords).|
|yaklaşımlar|Sıfır veya daha fazla yaklaşımları içerebilir. Daha ayrıntılı bilgi için bkz. [yaklaşımları](#sentiments).|
|audioEffects| Sıfır veya daha fazla audioEffects içerebilir. Daha ayrıntılı bilgi için bkz. [audioEffects](#audioeffects).|
|etiketleri| Sıfır veya daha fazla etiket içerebilir. Daha fazla ayrıntı için bkz [etiketleri](#labels).|
|markalar| Sıfır veya daha fazla markaları içerebilir. Daha ayrıntılı bilgi için bkz. [markaları](#brands).|
|İstatistikleri | Daha ayrıntılı bilgi için bkz. [istatistikleri](#statistics).|

## <a name="videos"></a>video

|Ad|Açıklama|
|---|---|
|Hesap Kimliği|Videonun VI hesap kimliği.|
|id|Videonun kimliği.|
|ad|Videonun adı.
|durum|Videonun durumuna (karşıya yüklenen, işleme, işlenen, başarısız, karantinaya alınmış).|
|processingProgress|İlerleme durumunu işlenirken (örneğin, % 20).|
|failureCode|(Örneğin, ' UnsupportedFileType') işlemi başarısız olursa hata kodu.|
|failureMessage|İşlem başarısız olursa hata iletisi.|
|externalId|(Kullanıcı tarafından belirtilmişse) videonun dış kimliği.|
|externalUrl|(Kullanıcı tarafından belirtilmişse) videonun dış url.|
|meta veriler|(Kullanıcı tarafından belirtilmişse) videonun dış meta verileri.|
|isAdult|Video el ile inceleme ve yetişkinlere yönelik bir video olarak tanımlanmış olup olmadığını gösterir.|
|görüşler|Insights nesne. Daha fazla bilgi için [ınsights](#insights).|
|thumbnailUrl|Video küçük resmi tam URL'si. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO..". Videonun özel ise, bir saat erişim belirteci URL'sini içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [accessToken]').|
|publishedUrl|Video akışı için bir url.|
|publishedUrlProxy|Videodan (Apple cihazlar için) akış URL'si.|
|viewToken|Video akışı için bir kısa süreli görünümü belirteci.|
|sourceLanguage|Video kaynak dili.|
|Dil|Videonun gerçek dili (çeviri).|
|indexingPreset|Videonun dizini oluşturmak için kullanılan hazır.|
|streamingPreset|Videoyu yayımlama için kullanılan hazır.|
|linguisticModelId|CRI model video özelliği kullanılır.|
|İstatistikleri | Daha fazla bilgi için [istatistikleri](#statistics).|

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

Öngörüler (örneğin, döküm satırları, yüzleri, markalar, vb.), burada her boyut (örneğin, face1 face2, Yüz3) benzersiz öğelerin listesini ve her öğe kendi meta verileri ve onun örneklerinin listesini sahip boyutları kümesidir (olduğu zaman aralığı ile ek isteğe bağlı meta veriler).

Yüz kimliği, bir ad, bir küçük resim, diğer meta veriler ve zamana bağlı örneklerinin listesini olabilir (örneğin: 00:00:05: 00:00:10, 00:01:00 - 00:02:30-00:41:21: 00:41:49.) Zamana bağlı her örneği, ek meta veri olabilir. Örneğin, yüzünün dikdörtgen (20,230,60,60) düzenler.

|Sürüm|Kod sürümü|
|---|---|
|sourceLanguage|Video kaynak dili (bir ana dil varsayılarak). Biçiminde bir [BCP-47](https://tools.ietf.org/html/bcp47) dize.|
|Dil|Insights dili (kaynak dili çevrilir). Biçiminde bir [BCP-47](https://tools.ietf.org/html/bcp47) dize.|
|transkript|[Döküm](#transcript) boyut.|
|OCR|[Ocr](#ocr) boyut.|
|anahtar sözcükler|[Anahtar sözcükleri](#keywords) boyut.|
|blokları|Bir veya daha fazla içerebilir [blokları](#blocks)|
|yüzleri|[Yüzleri](#faces) boyut.|
|etiketleri|[Etiketleri](#labels) boyut.|
|anlık görüntüleri|[Görüntüleri](#shots) boyut.|
|markalar|[Markaları](#brands) boyut.|
|audioEffects|[AudioEffects](#audioEffects) boyut.|
|yaklaşımlar|[Yaklaşımları](#sentiments) boyut.|
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

#### <a name="blocks"></a>blokları

Öznitelik | Açıklama
---|---
id|Blok kimliği.|
örnekler|Bu bloğu zaman aralıklarının listesi.|

#### <a name="transcript"></a>transkript

|Ad|Açıklama|
|---|---|
|id|Satır kimliği|
|metin|Transkripti kendisi.|
|Dil|Döküm dili. Transkript desteklemek her satırı farklı bir dil sahip olduğu yöneliktir.|
|örnekler|Bu satırı nerede göründüğü zaman aralıkları listesi. Transkript örneğiyse yalnızca 1 örneğin olacaktır.|

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
|güven|Tanıma güvenilirlik.|
|Dil|OCR dili.|
|örnekler|Bu OCR nerede göründüğü zaman aralıkları listesi (aynı OCR birden çok kez görünebilir).|

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
|güven|Anahtar sözcüğü'nın tanıma güvenilirlik.|
|Dil|Anahtar sözcüğü (çevrildiğinde) dili.|
|örnekler|Bu anahtar sözcük nerede göründüğü zaman aralıkları listesi (bir anahtar sözcüğü, birden çok kez görünebilir).|

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

#### <a name="faces"></a>yüzleri

|Ad|Açıklama|
|---|---|
|id|Face ID|
|ad|Yüz tanıma adı. 'Bilinmeyen #0', tanımlanmış bir ünlü veya müşteri eğitilen kişi olabilir.|
|güven|Yüz tanıma güvenilirlik.|
|açıklama|Ünlü açıklaması. |
|thumbnalId|Yüz tanıma, küçük resim kimliği.|
|knownPersonId|Bilinen bir kişi, kendi iç kimliği ise|
|Başvuru Kimliği|Bir Bing ünlü Bing kimliği ise|
|referenceType|Şu anda yalnızca Bing.|
|başlık|Ünlü, alt başlık (örneğin "CEO Microsoft'un") ise.|
|ImageUrl|Ünlü, görüntü URL'sini ise.|
|örnekler|Bunlar örnekleri, burada belirtilen zaman aralığında yüzü göründü. Her örnek, bir thumbnailsId de vardır. |

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

#### <a name="labels"></a>etiketleri

|Ad|Açıklama|
|---|---|
|id|Etiket Kimliği|
|ad|Etiket adı (örneğin, 'Bilgisayara', 'TV').|
|Dil|Etiket adı (çevrildiğinde) dili. BCP-47|
|örnekler|Bu etiket nerede göründüğü zaman aralıkları listesi (bir etiket birden çok kez görünebilir). Her örnek güvenle alana sahiptir. |


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

#### <a name="shots"></a>anlık görüntüleri

|Ad|Açıklama|
|---|---|
|id|Görüntüsü kimliği.|
|ana kareler|Anahtar çerçeveler (her bir kimlik ve örnekleri zaman aralıkları listesi vardır) görüntüsü içindeki bir listesi. Anahtar çerçeveler örneğiniz kimliği thumbnailId alana ana kare'nın küçük resimle|
|örnekler|Bu görüntüsü zaman aralıkları listesi (yalnızca 1 örneğinin sahip görüntüleri).|

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

İş ve ürün marka konuşma metin dökümü ve/veya videoyu OCR algılandı adları. Bu, markaları veya logosu algılama visual tanıma içermez.

|Ad|Açıklama|
|---|---|
|id|Marka kimliği|
|ad|Markaları adı.|
|Başvuru Kimliği | Marka wikipedia URL'si son eki. Örneğin, "Target_Corporation" soneki eklenir [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | Marka, Wikipedia URL'si, kullanıcının bulunmaktadır. Örneğin, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|açıklama|Markaları açıklaması.|
|etiketler|Bu marka ile ilişkili önceden tanımlanmış Etiketler listesi.|
|güven|Video Indexer marka algılayıcısı (0-1) güvenle değeri.|
|örnekler|Bu marka zaman aralıklarının listesi. Her örnek, bu marka transkripti veya OCR görünen olup olmadığını gösteren bir brandType sahiptir.|

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
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
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

#### <a name="statistics"></a>İstatistikleri

|Ad|Açıklama|
|---|---|
|CorrespondenceCount|Videoda yazışmalar sayısı.|
|WordCount|Konuşmacı sözcük sayısı.|
|SpeakerNumberOfFragments|Parçaları miktarı bulunan bir videoyu Konuşmacı vardır.|
|SpeakerLongestMonolog|Konuşmacı uzun monolog. Konuşmacı silences monolog içinde varsa dahil edilir. Başında ve sonunda monolog sessizlik kaldırılır.| 
|SpeakerTalkToListenRatio|Hesaplama videonun toplam zaman bölünmüş konuşmacının monolog (olmadan arasındaki sessizlik) üzerinde harcanan zamanı temel alır. Saat üçüncü ondalık noktasına yuvarlanır.|

#### <a name="audioeffects"></a>audioEffects

|Ad|Açıklama|
|---|---|
|id|Ses efekti kimliği.|
|type|Ses efekti türü (örneğin, Clapping, okuma, sessizlik).|
|örnekler|Bu ses efekti nerede göründüğü zaman aralıkları listesi.|

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

Yaklaşımları sentimentType alanı (nötr/olumlu/olumsuz) tarafından toplanır. Örneğin, 0-0.1, 0.2 0,1.

|Ad|Açıklama|
|---|---|
|id|Yaklaşım kimliği.|
|Ortalama Not karşılaştırması |Bu yaklaşım türdeki - nötr/olumlu/olumsuz tüm örneklerin tüm puanları ortalaması|
|örnekler|Bu yaklaşım nerede göründüğü zaman aralıkları listesi.|
|sentimentType |Türü 'Pozitif', 'Nötr' veya 'Negatif' olabilir.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
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

Video Indexer, büyük olasılıkla yetişkinlere yönelik içeriğe sahip bulunan olan tarih aralıkları visualContentModeration blok içerir. VisualContentModeration boşsa, tanımlanan yetişkinlere yönelik içerik yok.

Yetişkinlere yönelik veya müstehcen içerikleri bulunan videoları yalnızca özel görünüm için olabilir. Kullanıcılar bir insan tarafından İnceleme çalışması IsAdult öznitelik insan tarafından İnceleme sonucunu içerecek içerik için bir istek göndermek seçeneğiniz vardır.

|Ad|Açıklama|
|---|---|
|id|Görsel içerik denetleme kimliği.|
|adultScore|Yetişkinlere yönelik içerik puanı (Başlangıç, content moderator).|
|racyScore|Müstehcenlik puanı (Başlangıç, içerik denetleme).|
|örnekler|Bu görsel içerik denetleme nerede göründüğü zaman aralıkları listesi.|

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
|id|Metinsel içerik denetleme kimliği.|
|bannedWordsCount |Yasaklı bir sözcük sayısı.|
|bannedWordsRatio |Sözcükleri toplam sayısına oranı.|


## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

Uygulamanıza pencere öğeleri hakkında daha fazla bilgi için bkz: [uygulamalarınıza ekleme Video Indexer pencere öğeleri](video-indexer-embed-widgets.md). 

