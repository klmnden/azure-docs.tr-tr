---
title: SDK'lar ve örnekler - Content Moderator, Python, Java, Node.js ve .NET
titlesuffix: Azure Cognitive Services
description: Content Moderator için SDK'ları ve örnekleri alma
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: fd71a48372bcdb459bb3b7509e9a9c5dba529555
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60607102"
---
# <a name="content-moderator-sdks-and-samples"></a>Content Moderator SDK'ları ve örnekler

## <a name="sdks-for-python-java-nodejs-and-net"></a>Python, Java, Node.js ve .NET için SDK'lar

- [Python için Content Moderator SDK'sı](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [Java için Content Moderator SDK'sı](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [Node.js için Content Moderator SDK'sı](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [.NET için Content Moderator SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>.NET SDK örnekleri

Aşağıdaki listede .NET için Azure Content Moderator SDK'sı kullanılarak derlenen kod örneklerine bağlantılar bulunmaktadır.

- **Yardımcı kitaplık**: [Content Moderator istemci kullanım için diğer örnekleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Bkz: [hızlı başlangıç](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Denetleme 
- **Görüntü denetimi**: [Yetişkinlere yönelik ve müstehcen içeriğin denetimi, metin ve yüz için bir görüntü değerlendirmek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz: [hızlı başlangıç](image-moderation-quickstart-dotnet.md).
- **Özel görüntüleri**: [Özel görüntü listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz: [hızlı başlangıç](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

- **Metin denetimi**: [Metin küfür ve kişisel veriler için ekran](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz: [hızlı başlangıç](text-moderation-quickstart-dotnet.md).
- **Özel terimleri**: [Özel terim listeleri ile orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz: [hızlı başlangıç](term-lists-quickstart-dotnet.md).

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

- **Video denetimi**: [Video yetişkinlere yönelik ve müstehcen içeriğin denetimi için tarama ve sonuçları elde](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz: [hızlı başlangıç](video-moderation-api.md).

### <a name="review"></a>Gözden geçirme
- **Görüntü işleri**: [Tarayan ve gözden geçirmeler oluşturan bir denetimi işi başlatmak](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz: [hızlı başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [İncelemeleri İnsan-de--döngü oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz: [hızlı başlangıç](moderation-reviews-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [Görüntü incelemeleri İnsan içinde--döngüsü için oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Video deşifre metni inceler**: [Video deşifre metni incelemeleri İnsan-de--döngü oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) bkz [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

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
