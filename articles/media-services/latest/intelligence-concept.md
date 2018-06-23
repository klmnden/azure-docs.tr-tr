---
title: Azure media Intelligence | Microsoft Docs
description: Azure Media Services'i kullanırken, AudioAnalyzerPreset ve VideoAnalyzerPreset kullanarak, ses ve video contnet analiz edebilirsiniz.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/24/2018
ms.author: juliako
ms.openlocfilehash: c488060b9db0ba482d12eee2394e5149b918950e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331529"
---
# <a name="media-intelligence"></a>Medya zekası

Azure Media Services REST v3 API, ses ve video içeriğine çözümlemek sağlar. İçeriğinizi çözümlemek için oluşturduğunuz bir **dönüştürme** ve gönderme bir **iş** bu hazır ayarlarından birini kullanır: **AudioAnalyzerPreset** veya **VideoAnalyzerPreset** . 

## <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

**AudioAnalyzerPreset** bir ses veya video dosyasından birden çok ses Öngörüler almanıza olanak sağlar. Çıktı bir JSON dosyası (içeren tüm) ve ses dökümü için VTT dosyası içerir. Bu hazır giriş dosyası dili biçiminde belirten bir özellik kabul eden bir [BCP47](https://tools.ietf.org/html/bcp47) dize. Ses Öngörüler şunları içerir:

* Ses transcription – zaman damgalı konuşulan sözcüklerin dökümü. Birden çok dil desteklenir
* Konuşmacı dizin oluşturma – hoparlöre ve karşılık gelen konuşulan sözcüklerin eşlemesi
* Konuşma düşünceleri analiz – ses transcription üzerinde gerçekleştirilen düşünceleri analiz çıktısı
* Anahtar sözcükler – ses transcription ayıklanır anahtar sözcükler.

## <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

**VideoAnalyzerPreset** video dosyasından birden çok ses ve video Öngörüler almanıza olanak sağlar. Çıktı bir JSON dosyası (içeren tüm), video dökümü ve küçük resimler koleksiyonu için bir VTT dosyası içerir. Bu hazır de kabul eden bir [BCP47](https://tools.ietf.org/html/bcp47) (video dilini temsil eden) dize özelliği olarak. Video Öngörüler yukarıda belirtilen ses Öngörüler ve aşağıdaki ek öğeler şunlardır:

* Yüz izleme – yüzeyleri videoda mevcut saat. Her yüz yüz kimliği ve karşılık gelen küçük resimler koleksiyonu vardır
* Görsel metin – optik karakter tanıma algılandığında metin. Zaman damgalı ve aynı zamanda anahtar sözcükler (ek olarak ses dökümü) çıkarmak için kullanılan metindir
* Ana kare – video ayıklanır ana kare koleksiyonu
* Görsel içerik yönetimini – Yetişkin veya yapısı saldırganlardan olarak işaretlenmiş videolar bölümü
* Ek açıklama – önceden tanımlanmış nesne modelini temel alan videolar yorumlama bir sonucu

##  <a name="insightsjson-elements"></a>insights.JSON öğeleri

Çıktı, ses veya video içinde bulunan tüm ınsights ile bir JSON dosyası (insights.json) içerir. Json aşağıdaki öğeleri içerebilir:

### <a name="transcript"></a>dökümü

|Ad|Açıklama|
|---|---|
|id|Satır kimliği.|
|metin|Dökümü kendisi.|
|Dil|Dökümü dili. Dökümü desteklemek her bir satır farklı bir dil sahip olduğu amaçlanmıştır.|
|örnekler|Burada bu satırı görünen zaman aralıkları listesi. Bir dökümü örneğiyse yalnızca 1 örneği gerekir.|

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

### <a name="ocr"></a>OCR

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

### <a name="keywords"></a>anahtar sözcükler

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

### <a name="faces"></a>yazıtipleri

|Ad|Açıklama|
|---|---|
|id|Yüz kimliği.|
|ad|Yüz adı. 'Bilinmeyen #0', tanımlanan ünlülerle veya müşteri eğitilen kişi olabilir.|
|Güven|Yüz kimliği güven.|
|açıklama|Bir ünlülerle durumunda açıklamasını. |
|thumbnalId|Bu yüz küçük resim kimliği.|
|knownPersonId|Bilinen kişi olması durumunda, kendi iç kimliği.|
|Referenceıd|Bir Bing ünlülerle durumunda, Bing kimliği|
|referenceType|Şu anda yalnızca Bing.|
|başlık|Bir ünlülerle durumunda başlığını (örneğin "CEO Microsoft'un").|
|ImageUrl|Bir ünlülerle durumunda resim URL'si.|
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

### <a name="labels"></a>etiketleri

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

### <a name="shots"></a>görüntüleri

|Ad|Açıklama|
|---|---|
|id|Görüntüsü kimliği.|
|ana kare|(Her bir kimlik ve örnekleri zaman aralıkları listesi vardır) görüntüsü içindeki anahtar çerçeveler listesi.|
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
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
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
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```


### <a name="sentiments"></a>yaklaşımlar

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Media Services ile video Çözümle](analyze-videos-tutorial-with-api.md)
