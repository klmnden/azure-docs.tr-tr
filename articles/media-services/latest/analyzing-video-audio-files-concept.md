---
title: Azure Media Services ile video ve ses dosyalarını analiz etme | Microsoft Docs
description: Azure Media Services hizmetini kullanırken, ses ve video contnet AudioAnalyzerPreset ve VideoAnalyzerPreset kullanarak çözümleyebilirsiniz.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/21/2019
ms.author: juliako
ms.openlocfilehash: 9154e5d58a36bde1827d63d11d57a77b4289a781
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64689366"
---
# <a name="analyzing-video-and-audio-files"></a>Video ve ses dosyalarını analiz etme

Azure Media Services v3 Video Indexer AMS v3 Çözümleyicisi hazır (Bu makalede açıklanan) aracılığıyla, video ve ses dosyalarını içgörü sağlar. Daha ayrıntılı içgörüler istiyorsanız doğrudan Video Indexer’ı kullanın. Video Indexer’ı ve Media Services çözümleyicisinin önceden belirlenmiş ayarlarını hangi durumlarda kullanacağınızı anlamak için [karşılaştırma belgesine](../video-indexer/compare-video-indexer-with-media-services-presets.md) bakın.

İçeriğinizi Media Services v3 hazır kullanarak çözümlemek için oluşturduğunuz bir **dönüştürme** ve gönderme bir **iş** bu hazır birini kullanır: [VideoAnalyzerPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#videoanalyzerpreset) veya **AudioAnalyzerPreset**. Aşağıdaki makalede nasıl yapılacağı açıklanır **VideoAnalyzerPreset**: [Öğretici: Azure Media Services ile videoları analiz etme](analyze-videos-tutorial-with-api.md).

> [!NOTE]
> Video veya Ses Çözümleyicisi önayarlarını kullanırken, hesabınızı 10 S3 Medya Ayrılmış Birimine sahip olacak şekilde ayarlamak için Azure portalı kullanın. Daha fazla bilgi için bkz. [Medya işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

## <a name="built-in-presets"></a>Yerleşik hazır

Media Services şu anda aşağıdaki yerleşik Çözümleyicisi hazır destekler:  

|**Önceden tanımlı ayar adı**|**Senaryo**|**Ayrıntılar**|
|---|---|---|
|[AudioAnalyzerPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#audioanalyzerpreset)|Ses analizi|Yapay ZEKA tabanlı analiz işlemleri konuşma transkripsiyonu dahil olmak üzere, önceden tanımlanmış bir dizi hazır geçerlidir. Şu anda hazır işleme içerikle tek bir dilde konuşma içeren tek bir ses kaydı destekler. Ses yükü için dil giriş 'dil etiketi-region' BCP-47 biçimi kullanarak belirtebilirsiniz. Desteklenen bir dil olan İngilizce ('en-US' ve 'en-GB'), İspanyolca ('es-ES ' ve 'es-MX'), Fransızca ("fr-FR"), İtalyanca ('it-IT'), Japonca ('ja-JP'), Portekizce ('pt-BR'), Çince ('zh-CN'), Almanca ('de-DE'), Arapça ('ar-ÖRN'), Rusça ('ru-RU'), Hintçe ('Merhaba-IN' ) ve Kore dili ('ko-KR').<br/><br/> Dil değilse veya belirtilen için null değerler, otomatik dil algılama ayarlarsanız algılanan ilk dili seçin ve ile seçilen dile dosyayı süresi boyunca işlem. Otomatik dil algılama özelliği, şu anda İngilizce, Çince, Fransızca, Almanca, İtalyanca, Japonca, İspanyolca, Rusça ve Portekizce destekler. Şu anda ilk dil algılandıktan sonra diller arasında dinamik geçiş desteklemiyor. Otomatik dil algılama özelliğini açıkça anlaşılabilir Konuşmayla sesli kayıtlar ile en iyi çalışır. Dil bulmak otomatik dil algılama başarısız olursa, döküm İngilizce'ye döner.|
|[VideoAnalyzerPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#videoanalyzerpreset)|Ses ve video analiz etme|Ses hem video öngörüleri (zengin meta veriler) ayıklar ve çıkaran bir JSON biçim dosyası. Yalnızca ses video dosyası işlenirken içgörü isteyip istemediğinizi belirtebilirsiniz. Daha fazla bilgi için [Çözümle video](analyze-videos-tutorial-with-api.md).|
|[FaceDetectorPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#facedetectorpreset)|Videoda, mevcut tüm yüzleri algılama|Bir video analiz edilirken, mevcut tüm yüzleri algılamak için kullanılması için ayarları açıklar.|

### <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

Hazır bir ses veya video dosyasından birden çok ses ınsights almanıza olanak sağlar. Çıkış bir JSON dosyası (öngörülerle tüm) ve ses transkripti VTT dosyası içerir. Bu hazır kabul diline giriş dosyası biçiminde belirten bir özelliği bir [BCP47](https://tools.ietf.org/html/bcp47) dize. Ses ınsights şunlardır:

* Ses tanıma – konuşulan kelimeleri zaman damgalı bir kopyası. Birden çok dil desteklenir
* Konuşmacı dizin oluşturma – konuşmacıların ve karşılık gelen konuşulan kelimeleri ilişkin bir eşleme
* Konuşma yaklaşım analizi – ses tanıma üzerinde gerçekleştirilen yaklaşım analizi çıkışı
* Anahtar sözcükler – ses tanıma ayıklanan anahtar sözcükleri.

### <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

Önceden ayarlanmış bir video dosyasından birden çok ses ve video öngörüleri ayıklamak sağlar. Çıktı (tüm Öngörüler) içeren bir JSON dosyası, video deşifre metni ve küçük bir koleksiyon için bir VTT dosyası içerir. Bu hazır de kabul eden bir [BCP47](https://tools.ietf.org/html/bcp47) (video dilini temsil eden) dize özelliği. Video içgörüleri yukarıda belirtilen tüm ses Öngörüler ve aşağıdaki ek öğeler şunlardır:

* Yüz tanıma izleme – yüzleri videoda mevcut zaman. Face ID ve Parçacıkların karşılık gelen bir koleksiyon her yüz tanıma sahip
* Görsel metin – optik karakter tanıma algılanan metin. Zaman damgalı ve aynı zamanda anahtar sözcükleri (ses transkripti) ek olarak ayıklamak için kullanılan metindir
* Ana kareleri – videodan ayıklanan ana kareleri koleksiyonu
* Görsel içerik denetimi – yetişkinlere yönelik veya müstehcen yapısı olarak işaretlenmiş videolar bölümü
* Ek açıklama – bir sonucu bir önceden tanımlanmış nesne modelini temel videoları yorumlama

##  <a name="insightsjson-elements"></a>insights.JSON öğeleri

Çıktı, video veya ses bulunamadı öngörü bir JSON dosyası (insights.json) içerir. Json'u aşağıdaki öğeleri içerebilir:

### <a name="transcript"></a>transkript

|Ad|Açıklama|
|---|---|
|id|Satır kimliği|
|metin|Transkripti kendisi.|
|language|Döküm dili. Transkript desteklemek her satırı farklı bir dil sahip olduğu yöneliktir.|
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

### <a name="ocr"></a>OCR

|Ad|Açıklama|
|---|---|
|id|OCR satır kimliği|
|metin|OCR metin.|
|güven|Tanıma güvenilirlik.|
|language|OCR dili.|
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

### <a name="faces"></a>yüzleri

|Ad|Açıklama|
|---|---|
|id|Face ID|
|name|Yüz tanıma adı. 'Bilinmeyen #0', tanımlanmış bir ünlü veya müşteri eğitilen kişi olabilir.|
|güven|Yüz tanıma güvenilirlik.|
|description|Ünlü açıklaması. |
|thumbnailId|Yüz tanıma, küçük resim kimliği.|
|knownPersonId|Bilinen bir kişi, kendi iç kimliği ise|
|Başvuru Kimliği|Bir Bing ünlü Bing kimliği ise|
|referenceType|Şu anda yalnızca Bing.|
|title|Ünlü, alt başlık (örneğin "CEO Microsoft'un") ise.|
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

### <a name="shots"></a>anlık görüntüleri

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

### <a name="statistics"></a>İstatistikleri

|Ad|Açıklama|
|---|---|
|CorrespondenceCount|Videoda yazışmalar sayısı.|
|WordCount|Konuşmacı sözcük sayısı.|
|SpeakerNumberOfFragments|Parçaları miktarı bulunan bir videoyu Konuşmacı vardır.|
|SpeakerLongestMonolog|Konuşmacı uzun monolog. Konuşmacı silences monolog içinde varsa dahil edilir. Başında ve sonunda monolog sessizlik kaldırılır.| 
|SpeakerTalkToListenRatio|Hesaplama videonun toplam zaman bölünmüş konuşmacının monolog (olmadan arasındaki sessizlik) üzerinde harcanan zamanı temel alır. Saat üçüncü ondalık noktasına yuvarlanır.|


### <a name="sentiments"></a>yaklaşımları

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

### <a name="labels"></a>etiketleri

|Ad|Açıklama|
|---|---|
|id|Etiket Kimliği|
|name|Etiket adı (örneğin, 'Bilgisayara', 'TV').|
|language|Etiket adı (çevrildiğinde) dili. BCP-47|
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

### <a name="keywords"></a>anahtar sözcükler

|Ad|Açıklama|
|---|---|
|id|Anahtar sözcük kimliği.|
|metin|Anahtar sözcüğü metin.|
|güven|Anahtar sözcüğü'nın tanıma güvenilirlik.|
|language|Anahtar sözcüğü (çevrildiğinde) dili.|
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
## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Azure Media Services ile videoları analiz etme](analyze-videos-tutorial-with-api.md)
