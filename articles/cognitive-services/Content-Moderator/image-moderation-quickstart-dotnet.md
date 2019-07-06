---
title: 'Hızlı Başlangıç: Uygunsuz içeriği görüntüleri analiz ederek C# -Content Moderator'
titlesuffix: Azure Cognitive Services
description: .NET için içerik Moderator SDK'sını kullanarak çeşitli uygunsuz malzemeler için görüntü içeriği analiz etme
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: sajagtap
ms.openlocfilehash: d9a28dcd4af68cf9c00eb3d338c4bd83c8d89ecc
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604127"
---
# <a name="quickstart-analyze-images-for-objectionable-content-in-c"></a>Hızlı Başlangıç: Uygunsuz içeriği görüntüleri analiz edinC#

Bu makalede, [.NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için Content Moderator SDK'sını kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanır. Yetişkinlere yönelik veya müstehcen içerik, ayıklanan metin ve potansiyel olarak içeriklere yönetme bir yandan İnsan yüzlerini taraması öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- Content Moderator abonelik anahtarı. Content Moderator'a abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergelerini izleyin.
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü


> [!NOTE]
> Bu kılavuzda ücretsiz katmanı Content Moderator aboneliği kullanılır. Her abonelik katmanında nelerin sağlandığı hakkında bilgi için [Fiyatlandırma ve sınırlar](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/) sayfasına bakın.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

1. Visual Studio'da yeni bir **Konsol uygulaması (.NET Framework)** projesi oluşturun ve projeyi **ImageModeration** olarak adlandırın. 
1. Çözümünüzde başka projeler de varsa, tek başlangıç projesi olarak bunu seçin.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**'i seçin; ardından aşağıdaki projeleri bulun ve yükleyin:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-image-moderation-code"></a>Görüntü denetimi kodu ekleme

Ardından, temel bir içerik moderasyonu senaryosu uygulamak için kodu bu kılavuzdan kopyalayıp projenize yapıştıracaksınız.

### <a name="include-namespaces"></a>Ad alanlarını ekleme

Aşağıdaki `using` deyimlerini *Program.cs* dosyanızın en üstüne ekleyin.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=1-8)]

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisini oluşturma

Aboneliğinize bir Content Moderator istemci sağlayıcısı oluşturmak için aşağıdaki kodu *Program.cs* dosyanıza ekleyin. Aynı ad alanına **Program** sınıfıyla birlikte kodu ekleyin. **AzureRegion** ve **CMSubscriptionKey** alanlarını bölge tanımlayıcınızın ve abonelik anahtarınızın değerleriyle güncelleştirmeniz gerekecektir.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=84-107)]


### <a name="set-up-input-and-output-targets"></a>Giriş ve çıkış hedeflerini ayarlama

Aşağıdaki statik alanları _Program.cs_ dosyasındaki **Program** sınıfına ekleyin. Bu alanlar, giriş görüntü içerik için dosyaları belirtin ve JSON içeriği çıktı.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=49-53)]

*_ImageFiles.txt* giriş dosyasını oluşturmanız ve yolunu uygun şekilde güncelleştirmeniz gerekir (göreli yollar, yürütme dizinine göre belirlenir). _ImageFiles.txt_ dosyasını açın ve denetlenecek görüntülerin URL'lerini ekleyin. Bu hızlı başlangıçta örnek giriş olarak aşağıdaki URL'ler kullanılır.

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="create-a-class-to-handle-results"></a>Sonuçları işlemek için bir sınıf oluşturma

Aynı ad alanında *Program* sınıfıyla birlikte **Program.cs** dosyasına aşağıdaki kodu ekleyin. Gözden geçirilen görüntülerin her birinin denetim sonuçlarını kaydetmek için bu sınıfın bir örneğini kullanacaksınız.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=109-124)]


### <a name="define-the-image-evaluation-method"></a>Görüntü değerlendirme yöntemini tanımlama

**Program** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem, tek bir görüntüyü üç farklı yoldan değerlendirir ve değerlendirme sonuçlarını döndürür. Bu tek tek işlemlerin ne yaptığı hakkında daha fazla bilgi edinmek istiyorsanız, [Sonraki adımlar](#next-steps) bölümündeki bağlantıyı izleyin.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=55-81)]

### <a name="load-the-input-images"></a>Giriş görüntülerini yükleme

Aşağıdaki kodu **Program** sınıfındaki **Main** yöntemine ekleyin. Bu kod girdi dosyasındaki her bir resim URL'si için değerlendirme verileri almak üzere programı ayarlar. Ardından bu verileri tek bir çıkış dosyasına yazar.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=17-46)]

## <a name="run-the-program"></a>Programı çalıştırma

Program JSON dize verilerini _ModerationOutput.json_ dosyasına yazacaktır. Bu hızlı başlangıçta kullanılan örnek görüntüler aşağıdaki çıkışı verir. Her bir görüntü için farklı bölümlere sahip `ImageModeration`, `FaceDetection`, ve `TextDetection`, karşılık üç API çağrısı, **EvaluateImage** yöntemi.

```json
[{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
  "ImageModeration": {
    "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
    "adultClassificationScore": 0.019196987152099609,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.032390203326940536,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "116"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "12"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
    "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
    "language": "eng",
    "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
    "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
    "result": false,
    "count": 0,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "11"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "faces": []
  }
},
{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
  "ImageModeration": {
    "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
    "adultClassificationScore": 0.0035635426174849272,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.021369094029068947,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "109"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "46"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
    "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
    "language": "eng",
    "text": "",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
    "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
    "result": true,
    "count": 6,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "60"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
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
      },
      {
        "bottom": 575,
        "left": 594,
        "right": 773,
        "top": 396
      },
      {
        "bottom": 563,
        "left": 812,
        "right": 955,
        "top": 420
      },
      {
        "bottom": 611,
        "left": 972,
        "right": 1151,
        "top": 432
      },
      {
        "bottom": 510,
        "left": 1232,
        "right": 1456,
        "top": 286
      }
    ]
  }
}]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Content Moderator hizmetini kullanarak belirli bir görüntü örneği hakkındaki ilgili bilgileri döndüren basit bir .NET uygulaması geliştirdiniz. Bundan sonra, hangi verilere ihtiyacınız olduğuna ve uygulamanızın bunları nasıl işleyeceğine karar verebilmek için farklı bayrakların ve sınıflandırmaların anlamları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Görüntü moderasyonu kılavuzu](image-moderation-api.md)
