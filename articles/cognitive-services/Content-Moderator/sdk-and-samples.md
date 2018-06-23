---
title: İçerik yönetimini SDK'ları ve örnekleri için Azure içerik denetleyici | Microsoft Docs
description: SDK'ları ve örnekleri için içerik denetleyici alın
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: 40b8fc0f63383e837f0813f876f20806e6e8c6eb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351737"
---
# <a name="content-moderator-sdks-and-samples"></a>İçerik denetleyici SDK'ları ve örnekler

## <a name="sdks-for-python-java-nodejs-and-net"></a>Python, Java, Node.js ve .NET SDK'ları

- [İçerik denetleyici için Python SDK'sı](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [İçerik denetleyici için Java SDK'sı](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [İçerik denetleyici için Node.js SDK'sı](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [İçerik denetleyici için .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>.NET SDK'sı örneği

Aşağıdaki listede, .NET için Azure içerik denetleyici SDK kullanılarak oluşturulan kod örnekleri bağlantılarını içerir.

- **Yardımcı kitaplık**: [kullanmak için bir içerik denetleyici istemcisi diğer örnekleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Bkz: [Hızlı Başlangıç](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Denetleme 
- **Görüntü yönetimini**: [yazıtipleri, metin ve içeriği yetişkin ve saldırganlardan resmi değerlendirmek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz: [Hızlı Başlangıç](image-moderation-quickstart-dotnet.md).
- **Özel resimler**: [özel görüntü listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz: [Hızlı Başlangıç](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Maksimum sınırı yoktur **5 görüntü listeleri** her listesine ile **10.000 görüntüleri aşmaması**.
>

- **Metin denetleme**: [ekran uygunsuz metin ve kişisel bilgileri (PII) için metin](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz: [Hızlı Başlangıç](text-moderation-quickstart-dotnet.md).
- **Özel terimler**: [özel terim listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz: [Hızlı Başlangıç](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Maksimum sınırı yoktur **5 terim listeler** her listesine ile **10.000 koşulları aşmaması**.
>

- **Video yönetimini**: [yetişkin ve saldırganlardan içerik için bir video tarama ve sonuçları elde](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz: [Hızlı Başlangıç](video-moderation-api.md).

### <a name="review"></a>İncele
- **Görüntü işleri**: [tarayan ve incelemeler oluşturan bir denetleme işi başlatmak](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz: [Hızlı Başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeler**: [oluşturma incelemeler İnsan-içinde--döngü için](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz: [Hızlı Başlangıç](moderation-reviews-quickstart-dotnet.md).
- **Video incelemeleri**: [oluşturma video incelemeleri İnsan-içinde--for döngüsü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Video Dökümü incelemeleri**: [oluşturma video dökümü incelemeleri İnsan-içinde--for döngüsü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) görmek [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

Konumundaki tüm .NET örnekleri görmek [içerik denetleyici .NET örnekleri github'da](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

## <a name="rest-api-samples-in-c"></a>C# REST API örnekleri

- [Görüntü denetimi](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Metin denetimi](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Video denetimi](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Görüntü incelemeleri](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [Görüntü işleri](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

İzlenecek yollar Bu örnekler için kullanıma [isteğe bağlı Web Semineri](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html).

## <a name="tutorials"></a>Öğreticiler
- [e-ticaret katalog yönetimini](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Bkz: [Öğreticisi](ecommerce-retail-catalog-moderation.md).
- [Facebook içerik yönetimini](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Bkz: [Öğreticisi](facebook-post-moderation.md).
- [Video ve dökümü denetleme ve gözden geçirme çözümü](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) görmek [Öğreticisi](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>İsteğe bağlı Web Seminerlerini
- [Makine destekli içerik Denetleyici ile ölçekte içeriği denetleme](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
