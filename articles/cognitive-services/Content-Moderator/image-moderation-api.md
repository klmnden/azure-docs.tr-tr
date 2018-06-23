---
title: Azure içerik aracı - görüntü denetleme | Microsoft Docs
description: Uygunsuz görüntüleri Orta için görüntü yönetimini kullanın
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: c7cbc343c6e9113642d0ac79f4a4d60a404e8171
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355078"
---
# <a name="image-moderation"></a>Görüntü denetimi

İçerik denetleyicinin makine destekli görüntü yönetimini kullanın ve [İnsan gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md) yetişkin ve saldırganlardan içeriği görüntülerde Orta düzeye. Metin içeriği görüntüleri tarama metni ayıklayın ve yüzeyleri algılar. Başka bir işlem yapmanıza ve görüntüleri özel listeler karşı eşleşmiyor.

## <a name="evaluating-for-adult-and-racy-content"></a>Yetişkin ve saldırganlardan içerik için değerlendirme

**Değerlendir** işlemi, 0 ile 1 arasında bir güven puan döndürür. Bu da Boole veri eşit true veya false döndürür. Bu değerler, görüntüyü olası Yetişkin veya saldırganlardan içerik içerip içermediğini tahmin. Görüntü ile (dosya veya URL) API çağırdığınızda, döndürülen yanıt aşağıdaki bilgileri içerir:

    "ImageModeration": {
      .............
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      ............
      ],

> [!NOTE]

> - `isImageAdultClassified` cinsel açık veya bazı durumlarda yetişkinlere yönelik olarak kabul görüntüleri olası varlığını temsil eder.
> - `isImageRacyClassified` cinsel müstehcen veya bazı durumlarda yetişkin olarak kabul görüntüleri olası varlığını temsil eder.
> - Puanları 0 ile 1 arasında ' dir. Yüksek puanı kategori uygulanabilen yüksek modeli tahmin etmektir. Bu önizleme el ile kodlanmış sonuçlar yerine bir istatistik modeli kullanır. Her kategoride gereksinimlerinizi için nasıl hizalandığını belirlemek için kendi içerikle sınama öneririz.
> - Boole değerleri true veya false iç puan üzerinde eşikleri bağlı olarak. Müşteriler bu değeri kullanın ya da kendi içerik ilkelerine dayalı özel eşikler karar değerlendirmelisiniz.
>

## <a name="detecting-text-with-optical-character-recognition-ocr"></a>Metin ile optik karakter tanıma algılama

**Optik karakter tanıma** işlemi metin içeriği görüntüdeki varlığını tahmin ve diğer kullanımlar arasında metin denetleme için ayıklar. Dil belirtebilirsiniz. Bir dil belirtmezseniz, algılama İngilizce'ye varsayar.

Yanıt aşağıdaki bilgileri içerir:
- Özgün metin.
- GÜVENİRLİK puanlarını algılanan metin öğeleriyle.

Örnek Ayıkla:

    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      .........
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },


## <a name="detecting-faces"></a>Yüz algılama

Yüz algılama görüntüleri yüz gibi kişisel bilgileri (PII) algılamaya yardımcı olur. Olası yüzeyleri ve olası yüz sayısı her görüntüde algıla.

Bir yanıt bu bilgileri içerir:

- Yüz sayısı
- Algılanan yüzeyleri konumlarını listesi

Örnek Ayıkla:


    "FaceDetection": {
       ......
      "result": true,
      "count": 2,
      "advancedInfo": [
      .....
      ],
      "faces": [
        {
          "bottom": 598,
          "left": 44,
          "right": 268,
          "top": 374
        },
        {
          "bottom": 620,
          "left": 308,
          "right": 532,
          "top": 396
        }
      ]
    }

## <a name="creating-and-managing-custom-lists"></a>Özel listeleri oluşturma ve yönetme

Kullanıcılar görüntüleri veya başka türde bir içerik karşıya sonra birçok çevrimiçi topluluklarına rahatsız edici öğeleri birden çok kez aşağıdaki gün, hafta ve ay paylaşılan. Art arda tarama ve aynı görüntü veya birden fazla yerde görüntüden bile biraz değiştirilmiş sürümlerini filtreleme maliyetlerini pahalı ve hataya yatkın olabilir.

Birden çok kez aynı görüntü yönetme yerine rahatsız edici görüntüleri özel engellenen içerik listenize ekleyin. Bu şekilde, içerik yönetimini sisteminizi özel listelerinizi karşı gelen görüntüleri karşılaştırır ve herhangi başka bir işleme durdurur.

> [!NOTE]
> Maksimum sınırı yoktur **5 görüntü listeleri** her listesine ile **10.000 görüntüleri aşmaması**.
>

İçerik denetleyici eksiksiz [görüntü listesi Yönetimi API](try-image-list-api.md) özel resimler listesini yönetmek için işlemlerle. İle başlayan [görüntü listeler API konsol](try-image-list-api.md) ve REST API kod örnekleri kullanır. Ayrıca kullanıma [görüntü listesi .NET quickstart](image-lists-quickstart-dotnet.md) Visual Studio ve C# ile hakkında bilginiz varsa.

## <a name="matching-against-your-custom-lists"></a>Özel listelerinizi karşı eşleştirme

Oluşturulan ve yönetilen listeleme işlemleri kullanarak, özel listelerinizi hiçbirini karşı gelen görüntülerinin benzer eşleştirme eşleştirme işlemi sağlar.

Bir eşleşme bulunamazsa, işlem tanıtıcısı ve eşleşen görüntünün denetleme etiketleri döndürür. Yanıt, bu bilgileri içerir:

- (0 ve 1 arasında) eşleşmiyor puan
- Eşleşen görüntüsü
- Görüntü etiketleri (önceki denetleme sırasında atanır)
- Görüntü etiketleri

Örnek Ayıkla:

    {
    ..............,
    "IsMatch": true,
    "Matches": [
        {
            "Score": 1.0,
            "MatchId": 169490,
            "Source": "169642",
            "Tags": [],
            "Label": "Sports"
        }
    ],
    ....
    }

## <a name="human-review-tool"></a>İnsan tarafından inceleme aracı

Daha fazla bilgi için nuanced durumlarda içerik yöneticiyi kullanmak [aracı gözden](Review-Tool-User-Guide/human-in-the-loop.md) ve yönetimini sonuçları ve İnsan araburucu gözden içeriğinde yüzey API'si. Bunlar, atanmış makine etiketleri gözden geçirin ve son kararlarına onaylayın.

![İnsan araburucu için görüntü gözden geçirme](images/moderation-reviews-quickstart-dotnet.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Test sürücü [Görüntü Yönetimi API konsol](try-image-api.md) ve REST API kod örnekleri kullanır. Ayrıca kullanıma [görüntü yönetimi .NET quickstart](image-moderation-quickstart-dotnet.md) Visual Studio ve C# ile hakkında bilginiz varsa.
