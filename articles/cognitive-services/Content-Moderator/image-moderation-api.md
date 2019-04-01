---
title: Denetimi - Content Moderator resim
titlesuffix: Azure Cognitive Services
description: Content Moderator'ın makine Yardımlı resim denetimi ve orta görüntüleri için İnsan-de--döngü gözden geçirme aracı yetişkinlere yönelik ve müstehcen içeriği için kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 9f1df23d1f0f24787bb9267064ffd647eda2cb74
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756040"
---
# <a name="learn-image-moderation-concepts"></a>Görüntü denetimi kavramları öğrenin

Content Moderator'ın makine Yardımlı resim denetimi kullanın ve [İnsan içinde--döngüsü gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md) yetişkinlere yönelik ve müstehcen içerikleri için görüntüleri denetlemek. Metin içeriği için görüntü tarama, metin ayıklama ve yüzleri algılayın. Başka bir işlem yapmanıza ve özel listeler karşı görüntüleri eşleşmiyor.

## <a name="evaluating-for-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içeriğin denetimi için değerlendirme

**Değerlendir** işlem, 0 ile 1 arasında bir güven puanı döndürür. Ayrıca Boole veri eşittir true veya false döndürür. Bu değerler, görüntünün olası yetişkinlere yönelik veya müstehcen içerik içerip içermediğini tahmin edin. Görüntünüzü (dosya veya URL) ile API çağırdığınızda, döndürülen yanıt aşağıdaki bilgileri içerir:

    "ImageModeration": {
      .............
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      ............
      ],

> [!NOTE]
> 
> - `isImageAdultClassified`, belirli durumlarda açık saçık veya yetişkinlere yönelik olarak değerlendirilebilecek görüntülerin mevcut olma olasılığını gösterir.
> - `isImageRacyClassified`, belirli durumlarda davetkar veya yetişkinlere yönelik olarak değerlendirilebilecek görüntülerin mevcut olma olasılığını gösterir.
> - Puanları 0 ile 1 arasındadır. Yüksek puanı, kategori uygun olabilir yüksek modeli tahmin etmektir. Bu önizleme, el ile kodlanmış sonuçları yerine istatistiksel bir model kullanır. Her kategori için gereksinimlerinizi nasıl hizalandığını belirlemek için kendi içeriğe sahip test etmenizi öneririz.
> - Boole değerleri: true veya false iç puanına göre eşikleri bağlı olarak. Müşteriler, bu değeri kullanın veya kendi içerik ilkelere dayalı özel eşikler karar değerlendirmelisiniz.

## <a name="detecting-text-with-optical-character-recognition-ocr"></a>Metin ile optik karakter tanıma (OCR) algılama

**Optik karakter tanıma (OCR)** işlemi resimdeki metin içeriğini varlığını tahmin eder ve diğer kullanımları arasında metin denetimi için ayıklar. Dil belirtebilirsiniz. Bir dil belirtmezseniz, saptama İngilizce için varsayılan olarak.

Yanıt aşağıdaki bilgileri içerir:
- Orijinal metni.
- Güven puanlarını algılanan metin öğeleriyle.

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

Görüntüleri yüzleri gibi kişisel verileri algılamak için yüzleri algılama yardımcı olur. Her bir resim, olası yüzleri ve olası yüzlerin sayısı algılayın.

Yanıt, bu bilgiler şunları içerir:

- Yüz sayısı
- Yüz algılandı konumlarını listesi

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

Görüntüleri veya diğer türdeki bir içeriği, kullanıcıların karşıya yüklenmesinin ardından birçok çevrimiçi topluluklarına rahatsız edici öğeleri birden çok kez aşağıdaki gün, hafta ve ay paylaşılan. Sürekli tarama ve aynı görüntü veya birden fazla yerde görüntüden bile biraz değiştirilmiş sürümlerini filtreleme maliyetlerini, pahalı ve hata yapmaya açık olabilir.

Birden çok kez aynı görüntü yönetme yerine rahatsız edici görüntüleri özel engellenen içerik listenize ekleyin. Bu şekilde, içerik denetleme sistemi, özel listelerinizle gelen görüntüleri karşılaştırır ve herhangi başka bir işlemeye durdurur.

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

Content Moderator eksiksiz [görüntü listesi Yönetimi API'sini](try-image-list-api.md) özel görüntüleri listesini yönetmek için işlemleri. İle başlayan [görüntü listeleri API Konsolu](try-image-list-api.md) ve REST API kod örnekleri kullanın. Ayrıca kullanıma [görüntü listesi .NET Hızlı Başlangıç](image-lists-quickstart-dotnet.md) Visual Studio ve C# ile deneyiminiz varsa.

## <a name="matching-against-your-custom-lists"></a>Özel listelerinizi karşı eşleştirme

Eşleştirme işlemi, oluşturulan ve yönetilen listeleme işlemleri kullanarak, özel listeler hiçbirini karşı gelen görüntülerin benzer öğe eşleştirmesi sağlar.

Bir eşleşme bulunursa, işlem tanıtıcısı ve eşleşen görüntü denetimi etiketlerini döndürür. Yanıt, bu bilgiler şunları içerir:

- (0 ile 1 arasında) eşleştirme puanı
- Eşleşen görüntüsü
- Görüntü etiketleri (önceki denetimi sırasında atanır)
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

## <a name="human-review-tool"></a>İnsan inceleme aracı

Daha fazla bilgi için ince durumlarda Content Moderator'ı kullanın [İnceleme aracında](Review-Tool-User-Guide/human-in-the-loop.md) ve içeriği için İnsan, Moderatörler gözden geçirmesi ve denetleme sonuçları yüzey API'si. Bunlar, makine tarafından atanan etiketleri gözden geçirin ve son kararlarına onaylayın.

![İnsan denetimciler için görüntü incelemesi](images/moderation-reviews-quickstart-dotnet.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Sürücü test [görüntü denetim API'si konsol](try-image-api.md) ve REST API kod örnekleri kullanın. Ayrıca kullanıma [görüntü denetimi .NET hızlı](image-moderation-quickstart-dotnet.md) Visual Studio ve C# ile deneyiminiz varsa.
