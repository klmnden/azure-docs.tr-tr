---
title: Metin denetimi API'si - Content Moderator'ı kullanarak metin
titlesuffix: Azure Cognitive Services
description: Metin denetimi, çevrimiçi konsolda metin denetimi API'si kullanarak test edin.
services: cognitive-services
author: sanjeev3
ms.author: sajagtap
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: a3eb134d655f2a25acb45e0d249aa421667d1520
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621382"
---
# <a name="moderate-text-from-the-api-console"></a>API Konsolu Orta metni

Kullanım [metin denetimi API'si](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) metin içeriğinizi küfür tarama ve özel ve paylaşılan listeleriyle karşılaştırmak için Azure Content Moderator içinde.

## <a name="get-your-api-key"></a>API anahtarınızı alın

Çevrimiçi konsolunda API'yi test sürüşü önce abonelik anahtarınızı gerekir. Bu dosya çubuğunda bulunur **ayarları** sekmesinde **Ocp-Apim-Subscription-Key** kutusu. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="navigate-to-the-api-reference"></a>API başvuruya Git

Git [metin denetimi API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). 

  **Metin - ekran** sayfası açılır.

## <a name="open-the-api-console"></a>API Konsolu

İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

  ![Metin - ekran sayfası kayıt seçimi](images/test-drive-region.png)

  **Metin - ekran** API konsolu açılır.

## <a name="select-the-inputs"></a>Girişleri seçin

### <a name="parameters"></a>Parametreler

Metin ekranınızın kullanmak istediğiniz sorgu parametreleri seçin. Bu örnekte, varsayılan değeri kullanın **dil**. İşlem yürütme işleminin bir parçası otomatik olarak olasılığı dil algılar olduğundan aynı zamanda boş bırakılabilir.

> [!NOTE]
> İçin **dil** parametresi, Ata `eng` veya makine destekli görmek için boş bırakın **sınıflandırma** yanıt (Önizleme özelliği). **Bu özellik yalnızca İngilizce destekler**.
>
> İçin **küfür koşulları** algılama, kullanımı [ISO 639-3 kodu](http://www-01.sil.org/iso639-3/codes.asp) bu konuda listelenen desteklenen dillerin makale veya boş bırakın.

İçin **düzeltme**, **PII**, ve **(Önizleme) sınıflandırmak**seçin **true**. Bırakın **ListId** boş.

  ![Metin - ekran konsol sorgu parametreleri](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>İçerik türü

İçin **Content-Type**, ekran istediğiniz içerik türü seçin. Bu örnekte, varsayılan kullanmak **metin/düz** içerik türü. İçinde **Ocp-Apim-Subscription-Key** kutusuna, abonelik anahtarınızı girin.

### <a name="sample-text-to-scan"></a>Örnek metin taramak için

İçinde **istek gövdesi** kutusunda, metin girin. Aşağıdaki örnek, kasıtlı bir yazım yanlışı metni gösterir.

> [!NOTE]
> Aşağıdaki örnek metni geçersiz sosyal güvenlik numarası kasıtlıdır. Amacı, iletmek örnek giriş ve çıkış biçimi sağlamaktır.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
Also, 999-99-9999 looks like a social security number (SSN).
```

## <a name="analyze-the-response"></a>Yanıt analiz edin

Şu yanıtı çeşitli içgörüler API'den gösterir. Bu, olası küfürleri, kişisel verileri, sınıflandırma (Önizleme) ve otomatik olarak düzeltti sürümünü içerir.

> [!NOTE]
> Makine destekli 'Sınıflandırma' özellik Önizleme aşamasındadır ve yalnızca İngilizce dilini desteklemektedir.

```json
{"OriginalText":"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
"NormalizedText":"Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
"Misrepresentation":null,
"PII":{  
  "Email":[  
    {  
      "Detected":"abcdef@abcd.com",
      "SubType":"Regular",
      "Text":"abcdef@abcd.com",
      "Index":32
    }
  ],
  "IPA":[  
    {  
      "SubType":"IPV4",
      "Text":"255.255.255.255",
      "Index":72
    }
  ],
  "Phone":[  
    {  
      "CountryCode":"US",
      "Text":"6657789887",
      "Index":56
    },
    {  
      "CountryCode":"US",
      "Text":"870 608 4000",
      "Index":211
    },
    {  
      "CountryCode":"UK",
      "Text":"+44 870 608 4000",
      "Index":207
    },
    {  
      "CountryCode":"UK",
      "Text":"0344 800 2400",
      "Index":227
    },
    {  
      "CountryCode":"UK",
      "Text":"0800 820 3300",
      "Index":244
    }
  ],
  "Address":[  
    {  
      "Text":"1 Microsoft Way, Redmond, WA 98052",
      "Index":89
    }
  ],
  "SSN":[  
    {  
      "Text":"999999999",
      "Index":56
    },
    {  
      "Text":"999-99-9999",
      "Index":266
    }
  ]
},
"Classification":{  
  "ReviewRecommended":true,
  "Category1":{  
    "Score":1.5113095059859916E-06
  },
  "Category2":{  
    "Score":0.12747249007225037
  },
  "Category3":{  
    "Score":0.98799997568130493
  }
},
"Language":"eng",
"Terms":[  
  {  
    "Index":21,
    "OriginalIndex":21,
    "ListId":0,
    "Term":"crap"
  }
],
"Status":{  
  "Code":3000,
  "Description":"OK",
  "Exception":null
},
"TrackingId":"2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

JSON yanıtı tüm bölümlerinde ayrıntılı bir açıklaması için başvurmak [metin denetimi](text-moderation-api.md) kavramsal Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [metin denetimi .NET hızlı](text-moderation-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
