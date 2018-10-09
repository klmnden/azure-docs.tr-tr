---
title: SDK'lar ve örnekler - Content Moderator, Python, Java, Node.js ve .NET
titlesuffix: Azure Cognitive Services
description: Content Moderator için SDK'ları ve örnekleri alma
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: a57f6a312b00d7ec3d927c6fda319f1de8663c9c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220461"
---
# <a name="content-moderator-sdks-and-samples"></a>Content Moderator SDK'ları ve örnekler

## <a name="sdks-for-python-java-nodejs-and-net"></a>Python, Java, Node.js ve .NET için SDK'lar

- [Python için Content Moderator SDK'sı](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [Java için Content Moderator SDK'sı](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [Node.js için Content Moderator SDK'sı](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [.NET için Content Moderator SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>.NET SDK örnekleri

Aşağıdaki listede .NET için Azure Content Moderator SDK'sı kullanılarak derlenen kod örneklerine bağlantılar bulunmaktadır.

- **Yardımcı kitaplık**: [Diğer örneklerde kullanmak üzere bir Content Moderator istemcisi oluşturun](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Bkz: [hızlı başlangıç](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Denetleme 
- **Görüntü denetimi**: [Bir görüntüyü yetişkinlere yönelik ve müstehcen içerik, metin ve yüzler için değerlendirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz: [hızlı başlangıç](image-moderation-quickstart-dotnet.md).
- **Özel görüntüler**: [Özel görüntü listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz: [hızlı başlangıç](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Listesi sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 listedir**.
>

- **Metin denetimi**: [Metinde küfür ve kişisel bilgiler (PII) arama](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz: [hızlı başlangıç](text-moderation-quickstart-dotnet.md).
- **Özel terimler**: [Özel terim listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz: [hızlı başlangıç](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Üst sınır, her biri **10.000 terimi aşmamak** kaydıyla **5 listedir**.
>

- **Video denetimi**: [Videoda yetişkinlere yönelik ve müstehcen içerik arama ve sonuçları getirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz: [hızlı başlangıç](video-moderation-api.md).

### <a name="review"></a>Gözden geçirme
- **Görüntü işleri**: [Tarama yapan ve gözden geçirmeler oluşturan bir denetim işi başlatma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz: [hızlı başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [Devrede insan araştıran gözden geçirmeler oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz: [hızlı başlangıç](moderation-reviews-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [Devrede insan araştıran video gözden geçirmeleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Videoyu metne dönüştürme gözden geçirmeleri**: [Devrede insan araştırma için videoyu metne dönüştürme gözden geçirmeleri oluşturun](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

[GitHub'daki Content Moderator .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) arasında tüm .NET örneklerine bakın.

## <a name="rest-api-samples-in-c"></a>C# dilinde REST API örnekleri

- [Görüntü denetimi](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Metin denetimi](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Video denetimi](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Görüntü incelemeleri](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [Görüntü işleri](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

Bu örneklerin adımları için [isteğe bağlı Web seminerine](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html) bakın.

## <a name="tutorials"></a>Öğreticiler
- [E-Ticaret katalog denetimi](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Bkz: [öğretici](ecommerce-retail-catalog-moderation.md).
- [Facebook içerik denetimi](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Bkz: [öğretici](facebook-post-moderation.md).
- [Video ve metne dönüştürme denetimi ve gözden geçirme çözümü](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Bkz: [öğretici](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>İsteğe bağlı web seminerleri
- [Content Moderator ile uygun ölçekte makine destekli içerik denetimi](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
